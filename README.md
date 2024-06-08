# Tutorial: Procesamiento Automatizado de Imágenes para Contar Esporas en ImageJ con Macro # sporecount
## 1. Analizar una imagen
### Paso 1: Transformar a 8 bits
1.	Abre ImageJ.
2.	Carga tu imagen seleccionando File > Open... y elige tu imagen.
3.	Transforma la imagen a 8 bits yendo a Image > Type > 8-bit.
### Paso 2: Aplicar Umbral (Threshold)
1.	Ve a Image > Adjust > Threshold....
2.	Ajusta los valores de umbral moviendo los deslizadores hasta que las áreas de interés estén resaltadas en rojo.
3.	Haz clic en "Apply" para aplicar el umbral a la imagen.
### Paso 3: Aplicar Watershed
1.	Asegúrate de que la imagen esté en blanco y negro (las áreas de interés deben estar en blanco y el fondo en negro).
2.	Ve a Process > Binary > Watershed. Esta herramienta separará las partículas que están tocándose.
### Paso 4: Analizar Partículas
1.	Ve a Analyze > Set Measurements... y selecciona las siguientes opciones:
o	Area
o	Centroid
o	Circularity
2.	Haz clic en "OK" para guardar estas configuraciones.
3.	Ve a Analyze > Analyze Particles.... Configura los parámetros de la siguiente manera:
o	Size (píxeles cuadrados): 500-Infinity
o	Circularity: 0.50-1.00
o	Selecciona "Display Results".
o	Selecciona "Clear Results".
o	Selecciona "Summarize".
o	Selecciona "Add to Manager" si deseas guardar las partículas identificadas.
4.	Haz clic en "OK" para iniciar el análisis de partículas.
### Resultado
ImageJ procesará la imagen según los parámetros establecidos y mostrará una lista de las partículas identificadas con sus respectivos tamaños, circularidades y centroides.

## 2. Analizar una carpeta de imganes

### Paso 1: Crear un Macro para ImageJ
Abre ImageJ.
Ve a Plugins > New > Macro....
Copia y pega el siguiente código del macro en la ventana del editor de macros:

Guarda el macro con un nombre descriptivo como "Contar_Esporas_Macro.ijm".
Corre el macro haciendo clic en Run.

## Paso 2: Ejecutar el Macro en Múltiples Imágenes
Elige el directorio que contiene las imágenes de esporas que deseas procesar.
El macro procesará todas las imágenes en el directorio seleccionado y aplicará los siguientes pasos a cada una de ellas:
Transformar la imagen a 8 bits.
Aplicar umbral para resaltar las esporas.
Aplicar watershed para separar esporas que estén tocándose.
Analizar partículas con tamaño entre 500 e infinito y circularidad entre 0.50 y 1.
Los resultados, incluyendo el tamaño, circularidad y centroide de cada espora, se guardarán en archivos CSV en el mismo directorio.

## Ajustes y Consideraciones
Ajusta los valores de umbral en la línea setThreshold(50, 255); según sea necesario para tus imágenes específicas.
Asegúrate de que las imágenes estén en un formato compatible (.tif, .jpg, .png).
Verifica que las medidas de tamaño y circularidad sean apropiadas para las características de tus esporas.
¡Listo! Has creado y ejecutado un macro en ImageJ para contar esporas en múltiples imágenes de manera automatizada.

