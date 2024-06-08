********# Tutorial: Procesamiento Automatizado de Imágenes para Contar Esporas en ImageJ 

## 1. Analizar una imagen de esporas

<img src="https://github.com/ngmedina/sporecount/assets/9556792/aa4f7d58-0f38-4646-a274-4110ea0a17d1" alt="A_1_B06" width="500"/>

### Paso 1: Establecer la escala

1. Selecciona la herramienta de línea:
   - En la barra de herramientas de ImageJ, selecciona la herramienta de línea (línea recta).

2. Dibuja una línea sobre la barra de escala:
   - Haz clic y arrastra la herramienta de línea para dibujar una línea recta sobre la barra de escala en tu imagen. Asegúrate de que la línea coincida exactamente con la longitud de la barra de escala.

3. Medir la longitud de la línea:
   - Ve a `Analyze > Measure` o presiona Ctrl+M. Aparecerá una ventana de resultados que mostrará la longitud de la línea en píxeles. Anota este valor.

4. Abrir la herramienta "Set Scale":
Ve a `Analyze > Set Scale.... `
Configurar los valores en la ventana `Set Scale`:
   - En el campo "Distance in pixels", ingresa la longitud de la línea que mediste en píxeles.
   - En el campo "Known distance", ingresa la distancia real que la barra de escala representa (por ejemplo, 200 si la barra representa 200 micrómetros).
   - En el campo "Unit of length", ingresa la unidad de medida correspondiente (por ejemplo, "microns" o "µm").
   - Asegúrate de que la casilla "Global" esté marcada si deseas aplicar esta escala a todas las imágenes abiertas en la sesión actual.
5. Aplicar la escala:
   - Haz clic en `OK` para aplicar la escala.
Ejemplo
Supongamos que la barra de escala en tu imagen mide 400 píxeles y representa 200 micrómetros. Aquí se muestra cómo llenar los campos:

Distance in pixels: 400
Known distance: 200
Unit of length: microns
Esto significa que 400 píxeles en la imagen corresponden a 200 micrómetros en la realidad, por lo que cada píxel representa 0.5 micrómetros.

### Paso 3: Transformar a 8 bits

1.	Abre ImageJ.
2.	Carga tu imagen seleccionando `File > Open...` y elige tu imagen.
3.	Transforma la imagen a 8 bits yendo a `Image > Type > 8-bit`.

<img src="https://github.com/ngmedina/sporecount/assets/9556792/f218919a-8c80-4fc4-938d-e3a1d6305c10" alt="A_1_B06" width="500"/>


### Paso 4: Aplicar Umbral (Threshold)
1.	Ve a `Image > Adjust > Threshold....`
2.	Ajusta los valores de umbral moviendo los deslizadores hasta que las áreas de interés estén resaltadas en rojo.
3.	Haz clic en `Apply` para aplicar el umbral a la imagen.
   
<img src="https://github.com/ngmedina/sporecount/assets/9556792/8b4c532e-bd16-4da9-9c4b-21667107b3c2" alt="A_1_B06_threshold" width="700"/>

### Paso 5: Aplicar Watershed
1.	Asegúrate de que la imagen esté en blanco y negro (las áreas de interés deben estar en blanco y el fondo en negro).
2.	Ve a `Process > Binary > Watershed`. Esta herramienta separará las partículas que están tocándose.

<img src="https://github.com/ngmedina/sporecount/assets/9556792/5ce9e86f-4bae-47d6-a6c3-0c565c7268bd" alt="A_1_B06_watershed" width="500"/>

### Paso 6: Analizar Partículas

1.	Ve a `Analyze > Set Measurements...` y selecciona las siguientes opciones:
    -	Area
    -	Centroid
    - Circularity

2. Haz clic en `OK` para guardar estas configuraciones.

3.	Ve a `Analyze > Analyze Particles....` Configura los parámetros de la siguiente manera:
    - Size (píxeles cuadrados): 500-Infinity
    - Circularity: 0.50-1.00
    - Selecciona "Display Results".
    - Selecciona "Clear Results".
    - Selecciona "Add to Manager" si deseas guardar las partículas identificadas.

4.	Haz clic en `OK` para iniciar el análisis de partículas.

<img src="https://github.com/ngmedina/sporecount/assets/9556792/e6e80484-ffa7-41c6-9ec1-3d9d465e4dd9" alt="A_1_B06_particles" width="500"/>

### Resultado
ImageJ procesará la imagen según los parámetros establecidos y mostrará una lista de las partículas identificadas con sus respectivos tamaños, circularidades y centroides.

<img src="https://github.com/ngmedina/sporecount/assets/9556792/ccd314ad-2502-46e2-b4a1-bbbf9093009f" alt="A_1_B06_result" width="500"/>

## ¡Listo! Has analizado tu primera imagen para contar esporas en imageJ.

## 2. Analizar una carpeta de imganes

### Paso 1: Establece la escala de las imágenes

Observa la barra de escala en la imagen. En este caso, la barra indica que 200 micrómetros (µm) corresponden a una longitud específica en la imagen.
Mide la longitud de la barra de escala en píxeles utilizando la herramienta de línea en ImageJ `Analyze > Measure`.
Supongamos que la barra de escala mide 400 píxeles. Entonces, 400 píxeles corresponden a 200 µm.
Calcular la Escala:
La escala sería: 200 µm / 400 píxeles = 0.5 µm/píxel.
En el macro, ajusta la variable scale a este valor: scale = 0.5;.

### Paso 2: Ejecutar un Macro para ImageJ
Abre ImageJ.
Ve a `Plugins > New > Macro....`
Copia y pega el siguiente código y ``RECUERDA´´: tienes que actualizar el valor de escala

``` ruby
// Macro para procesar múltiples imágenes en ImageJ
// Transforma a 8 bits, aplica umbral, watershed, y analiza partículas

dir = getDirectory("Choose a Directory");
list = getFileList(dir);

// Define la escala (pixeles por unidad y unidad)
scale = 0.5;  // Número de píxeles por micrómetro (ajusta según tus datos)
unit = "microns";  // Unidad de medida (puede ser "microns", "mm", etc.)

for (i = 0; i < list.length; i++) {
    if (endsWith(list[i], ".tif") || endsWith(list[i], ".jpg") || endsWith(list[i], ".PNG" || endsWith(list[i], ".png")) {
        open(dir + list[i]);
        
        // Transformar a 8 bits
        run("8-bit");

        // Aplicar umbral (threshold)
        run("Auto Threshold", "method=Otsu white");

        // Aplicar Watershed
        run("Watershed");

        // Analizar partículas
        run("Set Measurements...", "area centroid circularity display redirect=None decimal=3");
        run("Analyze Particles...", "size=500-Infinity circularity=0.50-1.00 display exclude add");
        
        // Guardar resultados
        saveAs("Results", dir + "Results_" + list[i] + ".csv");
        
        // Cerrar imagen y resultados
        close();
        close("Results");
    }
}

```
Corre el macro haciendo clic en `Run`.

## Paso 2: Ejecutar el Macro en Múltiples Imágenes
1. Elige el directorio que contiene las imágenes de esporas que deseas procesar.
El macro procesará todas las imágenes en el directorio seleccionado y aplicará los siguientes pasos a cada una de ellas:
   - Transformar la imagen a 8 bits.
   - Aplicar umbral para resaltar las esporas.
   - Aplicar watershed para separar esporas que estén tocándose.
   - Analizar partículas con tamaño entre 500 e infinito y circularidad entre 0.50 y 1.
Los resultados, incluyendo el tamaño, circularidad y centroide de cada espora, se guardarán en archivos CSV en el mismo directorio.

## Ajustes y Consideraciones

Asegúrate de que las imágenes estén en un formato compatible (.tif, .jpg, .png, PNG).
Verifica que las medidas de tamaño y circularidad sean apropiadas para las características de tus esporas.

## ¡Listo! Has creado y ejecutado un macro en ImageJ para contar esporas en múltiples imágenes de manera automatizada.

