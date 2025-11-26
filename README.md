## ¿Qué es N-Grains?

**N-Grains** es una herramienta de segmentación automática para imágenes **2D**, diseñada especialmente para el análisis de **nanopartículas en imágenes TEM**.  
El sistema utiliza **Cellpose** como núcleo de segmentación, lo que permite trabajar con imágenes complejas manteniendo buenos resultados incluso cuando existen condiciones difíciles, como:

- Ruido en la imagen  
- Desenfoque (an)isotrópico  
- Submuestreo  
- Inversiones de contraste  
- Diferentes tamaños u órdenes de canales

---

## ¿Qué hace?

Además de la segmentación, **N-Grains amplía las capacidades de Cellpose**, permitiendo:

- Detección automática de cada nanopartícula en la imagen TEM  
- Generación de máscaras y contornos precisos  
- Cálculo automático de propiedades morfológicas como:

  - **Ancho**
  - **Largo**
  - **Diámetro equivalente**
  - **Conteo total de partículas**

- Visualización de resultados mediante:
  
  - **Histogramas**

---

## ¿Para qué sirve?

N-Grains permite caracterizar nanopartículas en microscopía TEM de forma:

- ✔ Rápida  
- ✔ Automatizada  
- ✔ Reproducible  
- ✔ Precisa  
- ✔ Escalable a grandes volúmenes de datos

Ideal para:

- Análisis de materiales  
- Caracterización de síntesis de nanopartículas  
- Evaluación morfológica  
- Procesamiento de datos de TEM en investigación científica

---



# Instalación

Para usar **N-Grains**, simplemente:

1. Descarga el notebook disponible en este repositorio (`N-Grains.ipynb`).
2. Súbelo a **Google Colab**.
3. Ejecútalo directamente desde la nube.

De esta forma no es necesario instalar librerías ni configurar el entorno en tu computadora.
Todo funciona desde Colab.

# GUI

El notebook incluye una interfaz gráfica interactiva, diseñada para analizar imágenes TEM de manera visual y sencilla.  
En esta sección se explica cómo usar la GUI:

----
<img width="380" height="380" alt="image" src="https://github.com/user-attachments/assets/5cc33f32-9215-48be-842b-2de38a38230b" />


Este bloque del código se encarga de configurar automáticamente el entorno de ejecución y preparar el modelo para su uso:
- Detecta si Colab dispone de **GPU** o **CPU**, y selecciona el mejor dispositivo disponible.
- Descarga el archivo del modelo entrenado (`Trained_model.cpn`) desde Google Drive.
- Carga el modelo personalizado en el **Código**.
----
Esta sección del código permite al usuario **subir una imagen* directamente desde su dispositivo y la prepara para ser procesada por el modelo:



<img width="500" height="949" alt="image" src="https://github.com/user-attachments/assets/9e183b9a-6df2-4c56-99b9-999f186f5f8f" />

----

### Mascaras
<img width="500" height="1013" alt="Captura de pantalla 2025-11-25 203832" src="https://github.com/user-attachments/assets/c71e8ee8-c423-4a2f-806d-1a61c5065235" />

## Parámetros de la segmentación

A continuación se detallan los controles principales de la interfaz y su efecto sobre la detección y delineado de los objetos en la imagen.

---

### 1. Umbral de detección (`cellprob_threshold`)

Controla la confianza mínima necesaria para que el modelo determine que una región corresponde a un objeto (por ejemplo, nanopartícula o célula).

- **Valores bajos (0.0 – 0.2):**
  - Acepta detecciones con baja certeza.
  - Más objetos detectados.
  - Mayor probabilidad de falsos positivos y ruido.

- **Valores altos (0.6 – 1.0):**
  - Solo se aceptan detecciones con alta probabilidad.
  - Menos objetos, pero detecciones más fiables y precisas.

Este parámetro actúa como un filtro final sobre la decisión del modelo.

---

### 2. Normalización (`tile_norm_blocksize`)

Controla cuánta normalización de brillo y contraste se aplica antes de procesar la imagen.  
Es útil para compensar iluminación desigual, muy común en imágenes TEM.

- **Valores bajos (0.0 – 0.2):**
  - Se aplica mínima corrección.
  - Útil cuando la iluminación es homogénea.

- **Valores altos (0.6 – 1.0):**
  - Normalización más fuerte.
  - Mejora la detección si el brillo está desequilibrado.
  - Si es demasiado alto, puede distorsionar el contraste original de la imagen.

---

### 3. Sensibilidad de bordes (`flow_threshold`)

Define qué tan fuertes deben ser los gradientes o bordes para ser considerados válidos al generar los contornos.

- **Valores bajos (0.0 – 0.3):**
  - Acepta bordes débiles.
  - Contornos más completos y detallados.
  - Puede introducir ruido.

- **Valores altos (0.7 – 1.0):**
  - Solo se aceptan bordes bien definidos.
  - Contornos más limpios y definidos.
  - Puede perder detalles sutiles.

Este parámetro permite controlar el balance entre detalle y limpieza visual.

---

### Sliders de selección de región
<img width="500" height="1013" alt="image" src="https://github.com/user-attachments/assets/f920075d-3f38-4c61-82d5-a297613531fe" />


La interfaz incluye cuatro controles para definir una región rectangular de análisis dentro de la imagen.  
Los valores están expresados en **porcentaje (%)**, por lo que funcionan con imágenes de cualquier tamaño.

- **Top (%)**  
  Límite superior.  
  Valores mayores recortan más desde arriba.

- **Bottom (%)**  
  Límite inferior.  
  Valores menores recortan desde abajo.

- **Left (%)**  
  Borde izquierdo.  
  Valores mayores recortan desde la izquierda.

- **Right (%)**  
  Borde derecho.  
  Valores menores recortan desde la derecha.

Internamente, cada porcentaje se convierte a coordenadas en píxeles y se filtran los objetos cuyo `bounding box` esté completamente dentro del rectángulo definido.  
Solo esos objetos:

- se muestran en la imagen,
- se miden (diámetro, ancho, largo, área, etc.),
- y se incluyen en el histograma resultante.

----
### Ajuste interactivo de escala

Imagen


Este bloque permite calibrar la escala de una imagen usando controles interactivos.  
El usuario mueve sliders para posicionar una barra de escala sobre una distancia real presente en la imagen.

### Controles

- **x_ini**: ajusta el borde izquierdo de la barra.  
- **x_fin**: ajusta el borde derecho.  
- **y_pos**: mueve la barra verticalmente.  
- **longitud_real**: distancia real conocida (ejemplo: `5 µm`, `500 nm`, `0.02 mm`).

### 📝 Cómo funciona

El usuario coloca la barra sobre la referencia real moviendo los sliders.  
La longitud ingresada se convierte internamente a la escala deseada (micrómetros, nanometros, etc) y se compara con la distancia en píxeles entre `x_ini` y `x_fin`.  
Con esto se obtiene la escala real por píxel.  

La interfaz muestra:
- la imagen completa con la barra de escala,
- un zoom para ajuste preciso,
- el valor final de la escala expresado en la unidad ingresada.

----
### Gráfica

Este bloque permite generar histogramas interactivos a partir de medidas procesadas (diámetros, anchos o largos), aplicar un ajuste gaussiano opcional y exportar la gráfica en formato PDF mediante un checkbox persistente.

### Controles principales

- **tipo_medida**: selecciona si el histograma usa datos de *centroide* o de *elipse*.  
- **usar_ancho / usar_largo**: aparecen solo cuando se elige *elipse* y permiten elegir qué dimensión usar.  
- **bin_method**: selección entre cálculo automático de bins (Sturges) o configuración manual.  
- **manual_bins**: número de bins cuando se usa el modo manual.  
- **show_fit**: activa o desactiva el ajuste gaussiano.  
- **titulo**: texto del título del gráfico.

### Funcionamiento

1. Se seleccionan los datos dependiendo del tipo de medida elegido.  
2. Las medidas se convierten automáticamente a unidades reales utilizando el valor obtenido durante la calibración de escala (`UU`).  
3. Se calcula la cantidad de bins usando el método de Sturges o el valor manual configurado.  
4. Se genera un histograma estilizado con Matplotlib.  
5. Si está activado el ajuste gaussiano, se calcula una curva de mejor ajuste y se muestra su media, desviación estándar y error.  
6. Se presenta un checkbox persistente **“Guardar como PDF”** que exporta la figura actual con los parámetros seleccionados.

### Guardar

El checkbox "Guardar como PDF" permite descargar directamente el histograma en alta resolución.  
Al activarlo:
- la figura actual se guarda como `histograma_exportado.pdf`,
- el archivo se descarga automáticamente,
- y el checkbox se reinicia para evitar exportaciones repetidas no deseadas.
---
## Citación

Este proyecto utiliza el modelo Cellpose como base para la segmentación automática de nanopartículas en imágenes TEM.  

### Cellpose-SAM
Pachitariu, M., Rariden, M., & Stringer, C. (2025).  
*Cellpose-SAM: superhuman generalization for cellular segmentation*. bioRxiv.  
Disponible en: https://www.biorxiv.org/content/10.1101/2025.04.28.651001v1

### Cellpose 3.0
Stringer, C., Wang, T., Michaelos, M., & Pachitariu, M. (2021).  
*Cellpose: a generalist algorithm for cellular segmentation*. Nature Methods, 18(1), 100–106.

> **Importante:** Aunque este repositorio adapta Cellpose para el análisis de nanopartículas (cálculo de diámetro, ancho, largo, conteo de objetos e histogramas en imágenes TEM), el modelo original pertenece a los autores mencionados y debe ser citado cuando se utilice en trabajos académicos, reportes o publicaciones.

