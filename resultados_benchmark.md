# Reporte de Benchmark y Respuestas: LLMOps Lab 1

Este documento detalla las respuestas completas obtenidas por cada modelo cuantizado frente a los prompts de prueba, acompañadas de sus métricas de rendimiento.

---

## Prueba 1: qwen2.5:1.5b-instruct-q4_K_M

- **Categoría de Prompt:** Corto (Definición)
- **Métricas:** `TTFT: 473.23 ms` | `Velocidad: 20.89 TPS` | `Tokens: 370` | `Tiempo Total: 18.2 s`

### Prompt Enviado:
> Explica en exactamente 3 viñetas qué es la cuantización en modelos de lenguaje.

### Respuesta Generada:
```text
La cuantización en modelos de lenguaje es un proceso fundamental en la generación de texto sintético y en la traducción automática. Aquí te explico en tres viñetas lo que es la cuantización:

1. **Interpretación de la Línea de Texto**: La cuantización comienza con la interpretación de la línea de texto que se está procesando. Esto implica la identificación de palabras, frases y oraciones individuales. En este paso, se trata de transformar la línea de texto en una representación más simple y estructurada.

2. **Selección de Palabras y Oraciones**: Una vez que se ha interpretado la línea de texto, se seleccionan las palabras y oraciones más relevantes para la tarea de cuantización. Esto puede implicar la eliminación de palabras o oraciones que no son necesarias para la traducción o la generación de texto sintético. La selección de palabras y oraciones se basa en la relevancia del contenido para el objetivo de la tarea.

3. **Transformación de la Representación**: Finalmente, se transforma la representación de la línea de texto en una forma más estructurada y compatible con el modelo de lenguaje utilizado. Esto puede implicar la aplicación de técnicas de cuantización, como la cuantización de palabras o la cuantización de oraciones, para convertir la representación en una forma más adecuada para la tarea específica.

En resumen, la cuantización es un proceso crucial en la generación de texto sintético y en la traducción automática, que implica la interpretación, selección y transformación de la representación de la línea de texto para adaptarla a la tarea específica.
```

---

## Prueba 2: qwen2.5:1.5b-instruct-q4_K_M

- **Categoría de Prompt:** Medio (Razonamiento)
- **Métricas:** `TTFT: 948.11 ms` | `Velocidad: 21.71 TPS` | `Tokens: 562` | `Tiempo Total: 26.84 s`

### Prompt Enviado:
> Tengo un servidor con 16 GB de RAM y sin GPU. Quiero desplegar un asistente interno para 5 empleados. ¿Qué arquitectura y tamaño de modelo me recomiendas y por qué?

### Respuesta Generada:
```text
Para un servidor con 16 GB de RAM y sin GPU, la elección de la arquitectura y tamaño de modelo para un asistente interno para 5 empleados puede ser un desafío. Sin embargo, puedo ofrecerte algunas sugerencias basadas en la capacidad de tu servidor y la necesidad de la aplicación:

1. Arquitectura: 
   - Arquitectura de chatbot: Esta es una buena opción para un asistente interno. Puedes usar una arquitectura basada en la conversación, como la de Anthropic (formerly Claude) o la de Anthropic (formerly Claude).
   - Arquitectura de IA: Si tu objetivo es crear un asistente más avanzado, podrías considerar una arquitectura de IA más compleja, como la de Anthropic (formerly Claude) o la de Anthropic (formerly Claude).

2. Tamaño de modelo:
   - Tamaño de modelo pequeño: Para un asistente interno para 5 empleados, un modelo pequeño podría ser suficiente. Esto podría incluir modelos como Claude 175M o Claude 300M.
   - Tamaño de modelo mediano: Si necesitas más capacidad para procesar más información o tener más opciones de personalización, podrías considerar modelos como Claude 700M o Claude 1.3B.
   - Tamaño de modelo grande: Si tu objetivo es crear un asistente más avanzado, podrías considerar modelos como Claude 3.0B o Claude 7.0B.

Recuerda que el tamaño de modelo también influye en la capacidad de tu servidor. Si tu servidor tiene 16 GB de RAM, un modelo de 700M o 1.3B podría ser un poco grande para tu servidor. Por lo tanto, si tu objetivo es crear un asistente más avanzado, podrías considerar un modelo de tamaño mediano o pequeño.

Además, ten en cuenta que la capacidad de tu servidor también influye en la elección del tamaño de modelo. Si tu servidor tiene menos RAM, podrías tener que elegir un modelo de tamaño más pequeño.

Por último, ten en cuenta que la elección del tamaño de modelo también puede influir en la velocidad de respuesta de tu asistente. Un modelo más grande puede tomar más tiempo para procesar una conversación, lo que podría afectar la velocidad de respuesta de tu asistente.

Recuerda que estas son solo sugerencias basadas en tu capacidad de servidor. Si tienes más información sobre tu proyecto, podrías necesitar ajustar estas sugerencias para obtener el mejor resultado posible.
```

---

## Prueba 3: qwen2.5:1.5b-instruct-q4_K_M

- **Categoría de Prompt:** Largo (Generación de Código)
- **Métricas:** `TTFT: 769.69 ms` | `Velocidad: 17.81 TPS` | `Tokens: 763` | `Tiempo Total: 43.62 s`

### Prompt Enviado:
> Escribe una función en Python bien documentada que implemente búsqueda binaria sobre una lista ordenada, incluyendo manejo de excepciones y 3 casos de prueba.

### Respuesta Generada:
```text
Aquí tienes una función bien documentada en Python que implementa la búsqueda binaria sobre una lista ordenada. Esta función incluye manejo de excepciones y 3 casos de prueba:

```python
def binary_search(arr, target):
    """
    Implementa la búsqueda binaria en una lista ordenada.

    Parámetros:
    arr (list): Lista ordenada en que se busca el target.
    target: El valor objetivo que se busca en la lista.

    Retorna:
    int: El índice del target si se encuentra en la lista, -1 si no se encuentra.

    Excepciones:
    ValueError: Si la lista no está ordenada o no es lista.
    TypeError: Si el target no es un número.
    """
    if not isinstance(arr, list):
        raise TypeError("El primer argumento debe ser una lista.")
    if not arr:
        raise ValueError("La lista no puede estar vacía.")
    if not all(isinstance(x, (int, float)) for x in arr):
        raise TypeError("Todos los elementos de la lista deben ser números.")
    if not arr:  # Verifica si la lista está vacía
        return -1

    left, right = 0, len(arr) - 1

    while left <= right:
        mid = (left + right) // 2
        mid_value = arr[mid]

        if mid_value == target:
            return mid
        elif mid_value < target:
            left = mid + 1
        else:
            right = mid - 1

    return -1

# Casos de prueba
print(binary_search([1, 2, 3, 4, 5], 3))  # Devuelve 2
print(binary_search([1, 2, 3, 4, 5], 6))  # Devuelve -1
print(binary_search([1, 2, 3, 4, 5], 1))  # Devuelve 0
print(binary_search([1, 2, 3, 4, 5], 5))  # Devuelve 4
print(binary_search([1, 2, 3, 4, 5], 0))  # Devuelve -1
print(binary_search([], 1))  # Devuelve -1
print(binary_search([1, 2, 3, 4, 5], 'a'))  # Devuelve -1
```

Esta función `binary_search` se implementa de la siguiente manera:

1. **Validación de entrada**: Comprueba si el primer argumento es una lista, si la lista está vacía, si todos los elementos de la lista son números, y si el target es un número.

2. **Búsqueda binaria**: Utiliza un bucle `while` para encontrar el índice del target en la lista. Si encuentra el target, devuelve su índice. Si no lo encuentra, devuelve -1.

3. **Casos de prueba**: Se implementan 3 casos de prueba para asegurarse de que la función funciona correctamente en diferentes escenarios:

   - Un caso de prueba con un target que se encuentra en la lista.
   - Un caso de prueba con un target que no se encuentra en la lista.
   - Un caso de prueba con un target que no es un número.
   - Un caso de prueba con una lista vacía.
   - Un caso de prueba con un target que no se encuentra en la lista.
   - Un caso de prueba con un target que no es un número.

Este código es bien documentado y incluye comentarios claros para explicar cada parte del código.
```

---

## Prueba 4: qwen2.5:1.5b-instruct-q8_0

- **Categoría de Prompt:** Corto (Definición)
- **Métricas:** `TTFT: 911.91 ms` | `Velocidad: 14.13 TPS` | `Tokens: 356` | `Tiempo Total: 26.11 s`

### Prompt Enviado:
> Explica en exactamente 3 viñetas qué es la cuantización en modelos de lenguaje.

### Respuesta Generada:
```text
1. **Definición y Concepto**: La cuantización en modelos de lenguaje es un proceso que reduce la cantidad de información en un texto o frase. Esto se logra al convertir el texto en un conjunto de números, donde cada número representa una palabra o una combinación de palabras. Este proceso se realiza para simplificar el texto y reducir su tamaño, lo que puede ser útil para la codificación, la transmisión de datos o la procesamiento de texto en entornos de aprendizaje automático.

2. **Aplicaciones y Beneficios**: En el contexto de modelos de lenguaje, la cuantización se utiliza para mejorar la eficiencia de la codificación de texto. Al reducir la cantidad de información, se puede ahorrar espacio en la memoria y mejorar la velocidad de procesamiento. Además, esta técnica puede ayudar a mejorar la precisión de los modelos de aprendizaje automático, ya que permite a los sistemas entender mejor el texto y predecir con mayor precisión las respuestas.

3. **Limitaciones y Consideraciones**: Aunque la cuantización puede mejorar la eficiencia de los modelos de lenguaje, también puede tener algunos efectos negativos. Por ejemplo, puede reducir la capacidad de los modelos para capturar la complejidad y la variabilidad del lenguaje humano. Además, la reducción de la cantidad de información puede afectar la capacidad de los modelos para entender contextos complejos o para predecir con precisión situaciones poco comunes. Por lo tanto, es importante encontrar un equilibrio entre la eficiencia y la capacidad de los modelos para capturar la complejidad del lenguaje.
```

---

## Prueba 5: qwen2.5:1.5b-instruct-q8_0

- **Categoría de Prompt:** Medio (Razonamiento)
- **Métricas:** `TTFT: 1564.51 ms` | `Velocidad: 13.94 TPS` | `Tokens: 642` | `Tiempo Total: 47.61 s`

### Prompt Enviado:
> Tengo un servidor con 16 GB de RAM y sin GPU. Quiero desplegar un asistente interno para 5 empleados. ¿Qué arquitectura y tamaño de modelo me recomiendas y por qué?

### Respuesta Generada:
```text
Para un servidor con 16 GB de RAM y sin GPU, hay varias arquitecturas y tamaños de modelos que podrían ser adecuadas para un asistente interno para 5 empleados. Aquí te dejo algunas opciones:

1. **Modelo de Conversación Simple**:
   - **Arquitectura**: Transformer
   - **Tamaño**: 110M o 345M
   - **Razón**: Los modelos de conversación simples son más eficientes en términos de memoria y pueden manejar una gran cantidad de datos de entrenamiento sin necesidad de GPU. Estos modelos son adecuados para tareas de chatbot o asistente interno.

2. **Modelo de Conversación Profesional**:
   - **Arquitectura**: Transformer
   - **Tamaño**: 774M o 1.2B
   - **Razón**: Estos modelos son más complejos y pueden manejar una gran cantidad de datos de entrenamiento. Son adecuados para tareas de chatbot o asistente interno que requieren una mayor capacidad de procesamiento y comprensión de contexto.

3. **Modelo de Conversación Profesional con GPU**:
   - **Arquitectura**: Transformer
   - **Tamaño**: 1.2B o 2B
   - **Razón**: Si tienes acceso a un GPU, estos modelos pueden ser más eficientes en términos de tiempo de entrenamiento y pueden manejar una gran cantidad de datos de entrenamiento. Sin embargo, estos modelos requieren más recursos y pueden ser más costosos.

4. **Modelo de Conversación Profesional con GPU y GPU Accelerated**:
   - **Arquitectura**: Transformer
   - **Tamaño**: 2B o 4B
   - **Razón**: Estos modelos son los más complejos y pueden manejar una gran cantidad de datos de entrenamiento. Sin embargo, requieren un GPU y un GPU Accelerated para entrenamiento y pueden ser más costosos.

5. **Modelo de Conversación Profesional con GPU y GPU Accelerated y GPU Accelerated**:
   - **Arquitectura**: Transformer
   - **Tamaño**: 4B o 8B
   - **Razón**: Estos modelos son los más complejos y pueden manejar una gran cantidad de datos de entrenamiento. Sin embargo, requieren un GPU, un GPU Accelerated y un GPU Accelerated para entrenamiento y pueden ser más costosos.

En general, si tienes un servidor con 16 GB de RAM y sin GPU, te recomendaría el modelo de conversación simple o profesional con GPU. Si tienes acceso a un GPU, te recomendaría el modelo de conversación profesional con GPU y GPU Accelerated. Si tienes acceso a un GPU y un GPU Accelerated, te recomendaría el modelo de conversación profesional con GPU y GPU Accelerated y GPU Accelerated.
```

---

## Prueba 6: qwen2.5:1.5b-instruct-q8_0

- **Categoría de Prompt:** Largo (Generación de Código)
- **Métricas:** `TTFT: 1365.13 ms` | `Velocidad: 13.97 TPS` | `Tokens: 655` | `Tiempo Total: 48.26 s`

### Prompt Enviado:
> Escribe una función en Python bien documentada que implemente búsqueda binaria sobre una lista ordenada, incluyendo manejo de excepciones y 3 casos de prueba.

### Respuesta Generada:
```text
Aquí tienes una función en Python que implementa la búsqueda binaria sobre una lista ordenada. La función está bien documentada y incluye manejo de excepciones y 3 casos de prueba.

```python
def binary_search(arr, target):
    """
    Realiza una búsqueda binaria en una lista ordenada.

    Parámetros:
    arr (list): Lista ordenada en la que se realizará la búsqueda.
    target: El valor objetivo que se busca en la lista.

    Retorna:
    int: El índice del elemento objetivo en la lista, o -1 si no se encuentra.

    Excepciones:
    ValueError: Si la lista no está ordenada.
    """
    if not arr:
        raise ValueError("La lista no puede estar vacía.")

    def binary_search_recursive(arr, target, low, high):
        if low > high:
            return -1

        mid = (low + high) // 2
        if arr[mid] == target:
            return mid
        elif arr[mid] > target:
            return binary_search_recursive(arr, target, low, mid - 1)
        else:
            return binary_search_recursive(arr, target, mid + 1, high)

    return binary_search_recursive(arr, target, 0, len(arr) - 1)


# Casos de prueba
def test_binary_search():
    # Caso 1: Lista ordenada, objetivo encontrado
    arr = [1, 2, 3, 4, 5]
    target = 3
    assert binary_search(arr, target) == 2

    # Caso 2: Lista ordenada, objetivo no encontrado
    arr = [1, 2, 3, 4, 5]
    target = 6
    assert binary_search(arr, target) == -1

    # Caso 3: Lista vacía
    arr = []
    target = 1
    try:
        binary_search(arr, target)
    except ValueError as e:
        assert str(e) == "La lista no puede estar vacía."

    print("Todos los casos de prueba pasaron exitosamente.")

# Ejecutar los casos de prueba
test_binary_search()
```

### Explicación de la función:
- **binary_search**: Esta es la función principal que realiza la búsqueda binaria. Recibe la lista `arr`, el objetivo `target`, y dos índices `low` y `high` para el rango de búsqueda.
- **binary_search_recursive**: Esta es una función auxiliar recursiva que se utiliza para realizar la búsqueda. Recibe la lista `arr`, el objetivo `target`, y los índices `low` y `high`.
- **Excepciones**: La función maneja `ValueError` si la lista está vacía.
- **Casos de prueba**: Se definen tres casos de prueba para verificar la funcionalidad de la función.

### Ejecución:
Para ejecutar los casos de prueba, simplemente ejecuta la función `test_binary_search()`. Si todos los casos pasan, se imprimirá "Todos los casos de prueba pasaron exitosamente."
```

---

