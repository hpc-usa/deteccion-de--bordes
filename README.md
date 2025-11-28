# Detección de Bordes con Algoritmo Sobel 🖼️

Implementación y comparación de algoritmos secuencial y paralelo para detección de bordes usando el operador Sobel en Python.

## 📋 Descripción

Este proyecto implementa el algoritmo de detección de bordes Sobel en dos versiones:
- **Versión Secuencial**: Procesamiento tradicional usando un único core de CPU
- **Versión Paralela**: Procesamiento distribuido usando múltiples cores con `multiprocessing`

El objetivo es demostrar el **speedup** (aceleración) logrado mediante el procesamiento paralelo en comparación con el enfoque secuencial tradicional.

## 🎯 Características

- ✅ Carga de imágenes desde URL o archivos locales
- ✅ Conversión automática a escala de grises
- ✅ Detección de bordes con operador Sobel 3x3
- ✅ Implementación secuencial (single-core)
- ✅ Implementación paralela (multi-core)
- ✅ Visualización comparativa de resultados
- ✅ Medición automática de tiempos de ejecución
- ✅ Cálculo del speedup obtenido

## 🔧 Requisitos

```bash
numpy
matplotlib
Pillow
requests
```

## 📦 Instalación

### Para Google Colab (Recomendado)
```python
# Las librerías ya están instaladas por defecto
# Solo copia y pega el código en una celda y ejecuta
```

### Para entorno local
```bash
pip install numpy matplotlib Pillow requests
```

## 🚀 Uso

### Uso Básico

```python
# Ejecutar con imagen de ejemplo por defecto
resultados = ejecutar_demo()
```

### Usar tu propia imagen

```python
# Desde URL
resultados = ejecutar_demo('https://ejemplo.com/imagen.jpg')

# Desde archivo local
resultados = ejecutar_demo('ruta/a/tu/imagen.jpg')
```

### En Google Colab - Subir archivo

```python
from google.colab import files
uploaded = files.upload()

# Usar el nombre del archivo subido
nombre_archivo = list(uploaded.keys())[0]
resultados = ejecutar_demo(nombre_archivo)
```

## 📊 Salida del Programa

El programa genera:

1. **Visualizaciones**:
   - Imagen original en color
   - Imagen en escala de grises
   - Bordes detectados (versión secuencial)
   - Bordes detectados (versión paralela)

2. **Métricas de rendimiento**:
   ```
   RESULTADOS DE RENDIMIENTO
   ======================================================================
   Tiempo Secuencial:  2.3456 seg
   Tiempo Paralelo:    0.6789 seg
   Speedup (S):        3.45x
   Cores utilizados:   4
   ======================================================================
   ```

3. **Diccionario de resultados**:
   ```python
   {
       'tiempo_secuencial': float,
       'tiempo_paralelo': float,
       'speedup': float,
       'imagen_original': numpy.ndarray,
       'imagen_gris': numpy.ndarray,
       'bordes_secuencial': numpy.ndarray,
       'bordes_paralelo': numpy.ndarray
   }
   ```

## 🧮 Fundamento Teórico

### Operador Sobel

El algoritmo Sobel utiliza dos kernels de convolución 3x3 para detectar cambios de intensidad:

**Kernel Gx (Bordes Verticales)**:
```
[-1  0  1]
[-2  0  2]
[-1  0  1]
```

**Kernel Gy (Bordes Horizontales)**:
```
[-1 -2 -1]
[ 0  0  0]
[ 1  2  1]
```

### Cálculo del Gradiente

Para cada píxel (i, j):
1. Se aplica convolución con ambos kernels
2. Se calcula la magnitud: **G = √(Gx² + Gy²)**
3. El valor G representa la intensidad del borde

## 🏗️ Estructura del Código

```
├── cargar_imagen()              # Carga imagen desde URL o archivo
├── rgb_a_escala_grises()        # Convierte RGB a escala de grises
├── mostrar_imagenes()           # Visualiza múltiples imágenes
├── sobel_secuencial()           # Algoritmo Sobel secuencial
├── procesar_fila_sobel()        # Procesa una fila (para paralelismo)
├── sobel_paralelo()             # Algoritmo Sobel paralelo
└── ejecutar_demo()              # Función principal de demostración
```

## ⚡ Optimización Paralela

La versión paralela divide el procesamiento de la imagen por filas, distribuyendo el trabajo entre todos los cores disponibles:

- Cada core procesa un subconjunto de filas de forma independiente
- Se utiliza `multiprocessing.Pool` para la distribución de tareas
- Los resultados se combinan al final para formar la imagen completa

**Speedup esperado**: Entre 2x y 4x dependiendo del número de cores disponibles.

## 📝 Ejemplo Completo

```python
import numpy as np
import matplotlib.pyplot as plt
from PIL import Image

# [... código del algoritmo ...]

# Ejecutar con imagen personalizada
url = 'https://ejemplo.com/mi-imagen.jpg'
resultados = ejecutar_demo(url)

# Acceder a los resultados
print(f"Speedup obtenido: {resultados['speedup']:.2f}x")

# Guardar imagen procesada
Image.fromarray(resultados['bordes_paralelo']).save('bordes_detectados.jpg')
```

