# Entregable Laboratorio 1: Benchmarking de Serving Local y Cuantización

- **Curso:** LLMOps, Serving Local y Arquitecturas RAG
- **Estudiante:** Valeria Sanchez Pizo
- **Hardware Utilizado:** AMD Ryzen 5, 15.28 GB, Windows 11 Pro
- **Fecha:** 12 de noviembre de 2026

---

## Análisis Técnico de Resultados

### 1. Impacto en Memoria (MEM USAGE y KV Cache)
> ¿Cuál fue la diferencia exacta en MB/GB de RAM consumida entre `q4_K_M` y `q8_0` al ejecutar `docker stats`? ¿Por qué el consumo en ejecución supera el tamaño del archivo en disco?

**Respuesta:**
Datos medidos con docker stats (límite 7.4 GiB):

Contenedor en reposo (sin modelo): 211.8 MiB
q4_K_M cargado: 1.294 GiB (~1325 MiB), es decir ~1113 MiB sobre la línea base.
q8_0 cargado: 1.908 GiB (~1954 MiB), es decir ~1742 MiB sobre la línea base.
Diferencia q8_0 − q4_K_M: 0.614 GiB (~629 MiB).

ollama ps confirmó 1.1 GB para q4_K_M y 1.8 GB para q8_0, ambos con 100% CPU y contexto 2048.

La diferencia de ~629 MiB coincide con la diferencia de pesos entre ambas cuantizaciones (~0.99 GB frente a ~1.6 GB en disco). El KV Cache y los buffers son iguales en ambos modelos, así que no alteran esa diferencia: solo cambia la parte de los pesos.

¿Por qué el consumo en ejecución supera el tamaño del archivo? Porque además de los pesos GGUF se reserva memoria para:

KV Cache: con num_ctx=2048 se reserva espacio para las claves y valores de atención de todas las capas. En Qwen2.5-1.5B (28 capas, 2 cabezas KV por GQA, dimensión 128, FP16) son ~28 KB por token, es decir ~56 MB para 2048 tokens. Es proporcional a num_ctx: duplicar el contexto duplicaría esta reserva, sin importar la cuantización.
Buffers de cómputo: activaciones, logits y memoria temporal de llama.cpp.
Overhead del runtime: el proceso de Ollama y su runner (el contenedor ya consumía ~212 MiB en reposo), más la caché de página de los pesos mapeados en memoria, que docker stats también contabiliza.

---

### 2. Ancho de Banda de Memoria y Velocidad (TPS)
> En inferencia sobre CPU, el cuello de botella suele ser el ancho de banda de la memoria RAM. ¿En qué porcentaje aumentó la velocidad (TPS) al pasar de 8 bits a 4 bits?

**Respuesta:**
Según mis resultados, pasar de 8 a 4 bits aumentó el TPS en promedio un ~43.7% (20.14 frente a 14.01 tok/s). Por prompt fue +47.8% en el corto, +55.7% en el medio y +27.5% en el largo.

En la generación autoregresiva cada token exige leer todos los pesos desde RAM. Un modelo q4 pesa ~40% menos que uno q8, así que se mueven menos bytes por token y sube el TPS.

La ganancia (~1.44×) es menor que la reducción teórica de bytes (~1.65×). Esto indica que en este CPU de 4 núcleos el límite no es solo el ancho de banda: la dequantización de q4_K_M y el cómputo de atención también pesan. Además, hubo variabilidad entre corridas, porque es un procesador de bajo consumo con posible límite térmico.

---

### 3. Latencia Inicial (TTFT) y Fase Prefill
> ¿Por qué el `TTFT (ms)` es significativamente más alto en el Prompt Largo que en el Prompt Corto?

**Respuesta:**
El TTFT del prompt largo (código) fue más alto que el del corto: 769.7 ms frente a 473.2 ms en q4 (+63%) y 1365 ms frente a 912 ms en q8 (+50%).

En la fase de prefill el modelo procesa todos los tokens del prompt en paralelo antes de emitir el primero. Esa fase está limitada por cómputo (multiplicaciones matriz-matriz), no por ancho de banda. Su costo crece con el número de tokens de entrada, y la atención crece de forma cuadrática. Además, la plantilla de chat agrega tokens fijos (system prompt).

El prompt largo es el de mayor salida esperada, pero su entrada es solo algo más extensa que la del corto. Por eso la diferencia es moderada y no un múltiplo grande. El prompt medio tuvo el TTFT más alto de todos (948 ms en q4 y 1565 ms en q8). Eso muestra que la longitud no explica todo y que también influyó la variabilidad del equipo. Además, q8 tuvo TTFT mayor que q4 en los tres prompts, porque el prefill también lee pesos más pesados.

---

### 4. Calidad vs. Eficiencia en `resultados_benchmark.md`
> Revisando las 6 respuestas completas generadas, ¿hubo alguna degradación perceptible en la lógica del código Python o en el razonamiento generado por el modelo de 4 bits frente al de 8 bits?

**Respuesta:**
Sí hubo degradación perceptible en q4, pero moderada. El techo de calidad lo pone el tamaño del modelo (1.5B), no la cuantización.

Código: ambos modelos implementan correctamente el algoritmo de búsqueda binaria. Pero q4 tiene inconsistencias internas:

Pidió 3 casos de prueba y generó 7, y su explicación lista 6.
Tiene código inalcanzable: valida if not arr con excepción y luego repite if not arr: return -1.
Sus comentarios son falsos: dice que binary_search([], 1) devuelve -1 cuando en realidad lanza ValueError, y que buscar 'a' devuelve -1 cuando lanzaría TypeError.
Su docstring promete validar el orden de la lista, pero no lo hace.

q8 entregó una versión recursiva coherente, con exactamente 3 pruebas con assert, aunque su explicación tiene un error menor: dice que binary_search recibe low y high.

Razonamiento: ambos fallan. q4 inventa modelos inexistentes (“Claude 175M”, “Anthropic (formerly Claude)”) y se contradice sobre qué tamaño cabe en 16 GB. q8 cae en repeticiones (“GPU Accelerated y GPU Accelerated…”) y recomienda configuraciones con GPU para un servidor sin GPU.

---
*Nota: Adjuntar junto con este documento los archivos generados `resultados_benchmark.csv` y `resultados_benchmark.md`.*
