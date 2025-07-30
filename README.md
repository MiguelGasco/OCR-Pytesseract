# Lector OCR de Facturas

Este proyecto implementa un sistema OCR (Reconocimiento Óptico de Caracteres) diseñado para extraer información clave de facturas, tanto en formato PDF como en imágenes (PNG, JPG, JPEG). Utiliza la librería `pytesseract` junto con `OpenCV` para el preprocesamiento de imágenes y la extracción de texto. Los datos extraídos se guardan en un archivo CSV para su fácil análisis y gestión.

## Características

* **Soporte Multi-formato**: Procesa facturas en formato PDF y varios formatos de imagen (PNG, JPG, JPEG).
* **Preprocesamiento de Imagen**: Incluye pasos de preprocesamiento como mejora de contraste, nitidez, conversión a escala de grises y umbralización para optimizar la precisión del OCR.
* **Extracción de ROI (Región de Interés)**: Define y extrae texto de áreas específicas de la factura (importe total, fecha, número de factura y nombre de la empresa).
* **Guardado en CSV**: Los datos extraídos se consolidan y se guardan en un archivo CSV, con la opción de añadir nuevos datos al mismo archivo.
* **Configuración Flexible**: Permite ajustar la configuración de Tesseract para diferentes modos de segmentación de página y motores OCR.
* **Herramienta Interactiva para ROI**: Incluye una función para seleccionar interactivamente las coordenadas de las ROI en una imagen, facilitando la adaptación a diferentes plantillas de factura.

## Requisitos

Para ejecutar este proyecto, necesitarás tener instalado lo siguiente:

* **Tesseract OCR**: El motor OCR en sí. Puedes descargarlo desde [aquí](https://tesseract-ocr.github.io/tessdoc/Installation.html). Asegúrate de añadir `tesseract.exe` a tu PATH o especificar la ruta en el código.
* **Poppler**: Necesario para que `pdf2image` pueda convertir PDFs a imágenes. Descárgalo desde [aquí](https://poppler.freedesktop.org/). Añade la carpeta `bin` de Poppler a tu PATH del sistema.

Además, las siguientes librerías de Python:

* `Pillow` (PIL)
* `pytesseract`
* `OpenCV` (cv2)
* `NumPy`
* `pdf2image`
* `pandas`

Puedes instalar las librerías de Python usando `pip`:

```bash
pip install Pillow pytesseract opencv-python numpy pdf2image pandas
```
# Configuración del Entorno

1.  **Instalar Tesseract OCR**:
    * Descarga e instala Tesseract desde el enlace proporcionado.
    * Asegúrate de que la ruta a `tesseract.exe` esté configurada en la variable `pytesseract.pytesseract.tesseract_cmd` en `app.ipynb`. Por ejemplo:

    ```python
    pytesseract.pytesseract.tesseract_cmd = r'C:\\Program Files\\Tesseract-OCR\\tesseract.exe'
    ```

2.  **Instalar Poppler**:
    * Descarga los binarios de Poppler desde el enlace proporcionado.
    * Descomprime el archivo y añade la ruta a la carpeta `bin` dentro de la carpeta descomprimida a tus variables de entorno del sistema (PATH).

# Uso

1.  **Organiza tus Facturas**: Coloca tus archivos de factura (PDF, PNG, JPG, JPEG) en una carpeta llamada `Facturas` en el mismo directorio que tu script `app.ipynb`.
2.  **Define las Regiones de Interés (ROI)**:
    * El diccionario `ROIS_COORDENADAS` en el script define las áreas de la imagen de donde se extraerá la información.
    * Las coordenadas están en el formato `(y1, y2, x1, x2)`.
    * Puedes usar la sección "OBTENER ROI INTERACTIVAMENTE" del notebook para identificar fácilmente estas coordenadas en tus facturas y ajustarlas si es necesario.
3.  **Ejecutar el Script**:
    * Abre el archivo `app.ipynb` en Jupyter Notebook o JupyterLab.
    * Ejecuta todas las celdas en orden.
    * El script procesará cada documento en la carpeta `Facturas` y guardará los datos extraídos en un archivo CSV llamado `factura_ejemplo.csv`.

# Estructura de ROIs

El diccionario `ROIS_COORDENADAS` predefinido es:

```python
ROIS_COORDENADAS = {
    'roi_importe': (335, 357, 193, 295),
    'roi_fecha': (335, 356, 86, 161),
    'roi_factura': (610, 628, 665, 736),
    'roi_empresa': (116, 135, 66, 281)
}
```
Estas coordenadas pueden necesitar ajustarse dependiendo del diseño de tus facturas.

### Modos de Segmentación de Página (PSM) y Motores OCR (OEM)

El script utiliza la configuración de Tesseract `CONFIG = '--psm 8 --oem 3 --psm 7'`. Puedes modificar esta cadena para optimizar el reconocimiento de texto.

* `--psm` (Page Segmentation Mode): Define cómo Tesseract interpreta la estructura de la página.
    * `7`: Trata la imagen como una sola línea de texto.
    * `8`: Trata la imagen como una sola palabra.
    * Consulta el notebook para una descripción completa de los modos PSM.
* `--oem` (OCR Engine Mode): Selecciona el motor OCR a usar.
    * `3`: Por defecto, basado en lo que está disponible (Legacy + LSTM).
    * Consulta el notebook para una descripción completa de los modos OEM.

### Funciones Clave

* `preprocesar_imagen(ruta_imagen)`: Carga una imagen, aplica mejoras de contraste y nitidez, convierte a escala de grises y umbraliza para preparar el texto para el OCR.
* `resize_roi(roi)`: Redimensiona una región de interés para mejorar la precisión del OCR.
* `extraer_texto_roi(imagen_umbral, roi_coordenadas, CONFIG)`: Extrae texto de una ROI específica utilizando Tesseract.
* `procesar_documento(ruta_documento)`: Orquesta el proceso de preprocesamiento y extracción de texto para un documento (PDF o imagen).
* `guardar_csv(datos, nombre_csv)`: Guarda los datos extraídos en un archivo CSV.
