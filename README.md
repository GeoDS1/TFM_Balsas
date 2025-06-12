# DETECCIÓN DE BALSAS DE AGUA EN IMÁGENES AÉREAS PNOA UTILIZANDO DEEP LEARNING CON YOLOv8n-OBB

Este repositorio contiene los notebooks de Jupyter que documentan el Trabajo de Fin de Máster (TFM) titulado "Detección de Balsas de Agua en Imágenes Aéreas PNOA Utilizando Deep Learning con YOLOv8n-OBB". El proyecto establece un flujo de trabajo integral desde el preprocesamiento de datos hasta la inferencia y evaluación de un modelo de Deep Learning para la detección automatizada de balsas.

## 📚 Palabras Clave (Keywords)

Detección de balsas, Imágenes aéreas PNOA, Deep Learning, YOLOv8n-OBB, Oriented Bounding Boxes (OBB), Preprocesamiento de datos vectoriales, Preprocesamiento de imágenes ráster, Análisis exploratorio de datos (EDA), Entrenamiento de modelo de detección, Inferencia con SAHI, Evaluación de modelo, Geoprocesamiento, TensorFlow/PyTorch, Google Colab, Cortes de imágenes (slicing), Filtrado espacial, Georreferenciación


La carpeta `Notebooks` incluye los cuadernos que detallan cada etapa del proyecto:

*   **01 Preprocesado de Datos Vectoriales:**
    Este notebook se enfoca en la **preparación y filtrado de datos vectoriales**. Incluye el preprocesamiento de los marcos de las imágenes PNOA IRG para definir el **ámbito de estudio**, la integración y filtrado de datos de la **Base Topográfica Nacional (BTN)**, y el refinamiento de la selección de balsas de riego y/o ganaderas utilizando información de la **REDIAM**. Finalmente, se aplica un filtrado por superficie mínima (200 m²) y se eliminan manualmente algunas geometrías no relevantes para obtener datos vectoriales 'limpios'.

*   **02 Preprocesado de Datos Raster:**
    El objetivo de este cuaderno es el preprocesamiento de las **imágenes ráster PNOA IRG**. Se optimizan las imágenes GeoTIFF COG originales extrayendo niveles de menor resolución (factor de reducción de 4, resultando en una **resolución de 1 metro por píxel** de los 0,25 m originales) para reducir la carga computacional. Se genera una versión reducida de todas las imágenes y se crea un **Virtual Raster (VRT)** que las combina en un único archivo virtual, facilitando el acceso y procesamiento unificado.

*   **03 Análisis Exploratorio de Datos (EDA):**
    En este notebook se realiza un Análisis Exploratorio de Datos (EDA) sobre el conjunto de imágenes aéreas y capas vectoriales. El objetivo es comprender la **distribución y características de las balsas**, así como su relación con los marcos de las imágenes. Se calculan **métricas geométricas** como área, perímetro y compacidad, y se visualiza su distribución espacial y densidad. El EDA reveló una **heterogeneidad en el tamaño y la forma de las balsas**, lo que representa un desafío para el modelo.

*   **04 Preparación de Datos de Entrenamiento:**
    Esta etapa se centra en la preparación de los datos para el entrenamiento del modelo YOLOv8n-obb. Se calculan los **Oriented Bounding Boxes (OBB)** para las geometrías de las balsas, que delimitan con mayor precisión su forma real. Se generan **cuadrados de recorte de tamaño fijo de 640x640 píxeles** que contienen las balsas, optimizados para el entrenamiento. Finalmente, se crean los **archivos de etiquetas en formato YOLOv8-OBB** con las coordenadas normalizadas de los vértices de los OBB.

*   **05 Preparación del Dataset y Entrenamiento con Yolov8:**
    En este cuaderno, se describe la **estructuración del dataset** para YOLOv8. Se organizan las imágenes y etiquetas en carpetas específicas para **entrenamiento (70%), validación (20%) y prueba (10%)**. Se configura el **archivo YAML** (`data.yaml`) que define las rutas y la lista de clases para el modelo. Se explica la elección de la arquitectura **YOLOv8n-obb** por su ligereza y soporte para OBB, y la estrategia de **transfer learning** con un modelo pre-entrenado. El entrenamiento se realiza en Google Colab debido a limitaciones de hardware local.

*   **06 Entrenamiento y Validación del Modelo (Google Colab):**
    Este notebook se ejecuta en Google Colab para aprovechar las **GPUs (Tesla T4)**, esenciales para acelerar el entrenamiento del modelo YOLOv8n-obb. Se detalla el proceso de entrenamiento, incluyendo parámetros como 100 épocas, un `batch size` de 8 y el optimizador AdamW. Los **resultados de validación** demuestran un rendimiento muy bueno:
    *   **mAP@0.5: 0.897 (89.7%)**
    *   **mAP@0.5:0.95: 0.574 (57.4%)**
    *   **Precisión (P): 0.883 (88.3%)**
    *   **Recall (R): 0.879 (87.9%)**

*   **07 Inferencia y Evaluación del Modelo:**
    Este cuaderno se centra en la aplicación del modelo entrenado a nuevas imágenes para realizar predicciones. Se utiliza la librería **SAHI (Slicing Aided Hyper Inference)** para optimizar la inferencia en imágenes de gran tamaño, dividiéndolas en "slices" y mejorando la detección de objetos pequeños. Las predicciones generadas se **convierten a geometrías georreferenciadas** (polígonos) y se **filtran espacialmente** para eliminar solapamientos con entidades geográficas que no son balsas (como embalses, lagunas, ríos, etc. de la cartografía BTN). Los resultados finales se visualizan en un mapa interactivo, mostrando que el modelo funciona bien con balsas medianas y pequeñas, aunque puede tener dificultades con balsas muy grandes. La inferencia sobre la imagen `0964_3` resultó en **106 objetos detectados y filtrados**.


