# myRetopoUV

Transferencia de color de un **Hi Poly** a las nuevas UV de un **Lo Poly**, para Windows, de **Pablo**. Carga los dos modelos (**GLB**, **FBX**, **OBJ**, **USDZ** o **USDA**) alineados en el mismo espacio 3D y proyecta el mapa del Hi Poly que elijas —color base, normal, rugosidad, metalicidad, altura, oclusión, emisivo u opacidad— sobre la malla de retopología. Las texturas que acompañan al modelo se detectan y agrupan solas, tanto con nombres tipo Substance Painter como incrustadas en el propio archivo. El cálculo se hace por **GPU con CUDA**: resoluciones de 256 a 8192, **antialiasing** de 1, 2, 4 u 8 muestras, **relleno (dilatación)** con margen configurable y soporte de **tiles UDIM**. Incluye dos visores 3D con órbita y zoom, dos vistas UV, un visor superpuesto con el recuento de polígonos, tris y vértices de cada modelo, y un indicador que muestra en todo momento si el cálculo se hace en GPU o en CPU. Funciona sin conexión.

## Descargar

**[Descargar la última versión](https://github.com/pabloY/myRetopoUV-descargas/releases/latest)** y, en el apartado *Assets*, elige:

- **`myRetopoUV-Setup-1.2.1-x64.exe`**: instalador (única descarga disponible; no hay versión portable).

**Requisitos:** Windows 10 u 11 de 64 bits y, para acelerar, una tarjeta **NVIDIA** con capacidad de cómputo **7.5 o superior**: GTX 16xx y todas las RTX. Un solo binario incluye código nativo para sm_75, sm_80, sm_86, sm_89, sm_90, sm_100 y sm_120, más PTX de reserva para arquitecturas futuras. No hace falta ser administrador, ni tener conexión a internet, ni instalar el CUDA Toolkit: del controlador de NVIDIA solo se usa el que ya tienes instalado.

**Sin GPU NVIDIA compatible** el programa funciona igual: el cálculo se hace en la CPU, con el mismo resultado y más tiempo. La cabecera de la ventana indica siempre qué motor se está usando, y en el caso de una GPU AMD avisa de que no se está aprovechando.

## Instalar

1. Descarga el instalador y ábrelo. Se instala **solo para tu usuario** y no pide permisos de administrador: no aparece el aviso de UAC. Copia los archivos a `%LOCALAPPDATA%\Programs\myRetopoUV` y crea accesos directos en el menú Inicio y en el escritorio.
2. **Aviso de Windows.** El instalador no tiene firma digital, así que es normal que:
   - el navegador pregunte si quieres conservar el archivo: elige **Conservar**;
   - Windows muestre «Windows protegió su PC»: pulsa **Más información → Ejecutar de todas formas**.
3. Para desinstalarlo: *Configuración → Aplicaciones → myRetopoUV → Desinstalar*, o el desinstalador que hay en la carpeta del programa.

Si quieres comprobar que la descarga está completa, compara su huella SHA-256 con la que da PowerShell:

```powershell
Get-FileHash .\myRetopoUV-Setup-1.2.1-x64.exe -Algorithm SHA256
```

La de la versión 1.2.1 es `3ee35635c3136dd7fd2832b003b4b614c779d7c4c59e3ab43e0fbd6e44c4a036`.

## Uso rápido

1. Carga el **Hi Poly** y el **Lo Poly** (GLB, FBX, OBJ, USDZ o USDA), alineados en el mismo espacio 3D. Las UV del Lo Poly deben ser únicas dentro de 0-1 o estar repartidas en tiles UDIM. Al abrir el Hi Poly se detectan solas las texturas que lo acompañan y aparecen en el selector **Mapa de textura**, agrupadas por tipo (Base Color, Normal, Roughness, Metallic, Metallic/Roughness, Height, Ambient Occlusion, Emissive, Opacidad y Specular).
2. Elige en **Mapa de textura** el mapa que quieres transferir. Los visores y las vistas UV muestran ese mapa, y **Calcular** transfiere al Lo Poly solo el que esté elegido, de modo que el mismo modelo sirve para generar la textura de color, la de normales y las demás. Según las tiles que contenga el Hi Poly aparecen además los botones **Cargar mapa 1001**, **Cargar mapa 1002**, **Cargar mapa 1011**… para sustituir o añadir texturas a mano en el mapa activo; el botón se pone en verde y muestra el nombre del archivo cuando ese UDIM ya tiene su mapa.
3. Ajusta **Resolución**, **Distancia de proyección** (en unidades del modelo), **Antialiasing** y el **Relleno / previsualización**:
   - *Sin relleno*: solo los píxeles que reciben proyección.
   - *Dilatación infinita*: extiende el color hasta el borde de la isla.
   - *Dilatación + transparente*, *+ color de fondo* o *+ difusión*: rellenan el margen con transparencia, con el color elegido o difuminando el color de los bordes.
   - **Margen (píxeles)**: ancho del relleno, de 0 a 256. El color de fondo se elige con un selector HSV con cuadrado de saturación y valor, barra de tono y campos HEX y RGB.
4. Pulsa **Calcular**. La barra de progreso muestra el avance por UDIM y el resultado aparece en el visor **Lo Poly**, junto al **Hi Poly**: arrastra para orbitar y usa la rueda para acercar. Las dos vistas inferiores muestran las UV de cada modelo, con su propio selector de UDIM en la barra y zoom, desplazamiento y doble clic para encuadrar.
5. **Guardar textura** abre un diálogo para elegir la carpeta de destino, con opción de crear una nueva. Se recuerda la última carpeta usada y se pide confirmación antes de reemplazar un archivo existente. El nombre sale del modelo y del mapa de origen y añade siempre el tipo de mapa y el UDIM, del estilo `Modelo_retopo_basecolor_1001.png`, de modo que los distintos mapas del mismo modelo no se pisan al exportarlos a la misma carpeta.

**Distribución de la ventana.** La interfaz está pensada para caber en **1920×1080 sin desplazamiento** en una rejilla de cuatro visores. Cada visor se maximiza con el **icono de su esquina superior derecha** o con **Alt+W** sobre el visor bajo el puntero, y se restaura repitiendo la acción o con **Escape**.

**Superponer.** El botón que hay encima de los dos visores 3D reúne el Hi Poly y el Lo Poly en un único visor, con el Lo Poly semitransparente y su malla en alambre, para comprobar de un vistazo que la retopología cae sobre la superficie original. El mismo botón, que pasa a llamarse **Salir de superposición**, devuelve la rejilla. En ese visor, los botones **HiPoly** y **LoPoly** de la cabecera ocultan o devuelven cada modelo por separado, para revisar uno de los dos sin el otro delante.

En la cabecera de cada visor, el rótulo **HiPoly**, **LoPoly**, **UV HiPoly** o **UV LoPoly** va seguido del nombre del archivo abierto, y en la esquina inferior derecha de los visores 3D y del superpuesto se lee el número de **polígonos, tris y vértices** del modelo cargado.

**Limitaciones conocidas:** el FBX debe ser estático (no se admiten esqueletos); en el FBX no se leen los MTL y las texturas se localizan por el nombre del archivo junto al modelo; el USD binario (.usdc) no está soportado y hay que exportar a .usda o .usdz; y las caras que cruzan el borde entre dos tiles UDIM se rechazan.

**Rendimiento orientativo** (205.000 triángulos, mapa de 2048, medido en una RTX 2060): a 2048 con AA 4 tarda 0,9 s por UDIM en GPU frente a unos 35 s en CPU; a 4096 la diferencia es de unos 3 s frente a casi 3 minutos. Por debajo de 1024 el arranque del motor pesa más que el cálculo y la ventaja se reduce.

## Historial de versiones

### 1.2.1

- **Créditos y licencias**: botón al pie de la barra lateral que abre un diálogo con la versión instalada, la autoría y el contacto, la dirección de este repositorio de descargas, la licencia de la aplicación y los avisos legales de los componentes de terceros: Electron, Chromium, three.js, fflate, NSIS y el runtime de NVIDIA CUDA.
- **Corregido**: el icono de maximizar de los dos visores 3D queda pegado a la esquina superior derecha, igual que en las vistas UV, en lugar de aparecer detrás del nombre del archivo.
- **Corregido**: los atajos de teclado no actúan con un diálogo abierto; Escape cierra el diálogo en vez de desmaximizar un visor a la vez.

### 1.2.0

- Los tres visores 3D muestran en su esquina inferior derecha el número de **polígonos, tris y vértices** del modelo, con los dos modelos separados por una barra en el visor superpuesto.
- En la superposición, dos botones (**HiPoly** y **LoPoly**) ocultan y devuelven cada modelo por separado.
- Al guardar, el nombre del archivo incluye siempre el **mapa** y el **UDIM**: `Modelo_retopo_basecolor_1001.png` en lugar de `Modelo_retopo_1001.png`, así que dos mapas del mismo modelo ya no se pisan.
- Los modelos se llaman **HiPoly** y **LoPoly**, en CamelCase, en los botones de apertura y en las cabeceras de los cuatro visores.
- El nombre del archivo abierto aparece detrás de ese rótulo, separado unos cuatro caracteres, en lugar de sustituirlo.
- El visor superpuesto deja de tener botón de maximizar, porque cuando está activo ya ocupa él solo toda el área de visores.

### 1.1.0

- **Superponer**: botón encima de los dos visores 3D que reúne ambos modelos en un solo visor, con el Lo Poly semitransparente y su malla en alambre, para comprobar que la retopología coincide con el Hi Poly. El mismo botón devuelve la rejilla.
- El **nombre del archivo abierto** en la cabecera de cada visor.
- Los dos campos de archivo de la barra lateral son ahora **botones**: desaparece el «Ningún archivo seleccionado» del control nativo.
- La etiqueta de los modelos pasa a «Hi Poly (GLB, FBX, OBJ, USD)».
- **Corregido**: las texturas incrustadas en los USDZ ya se cargan. La imagen se leía antes de estar decodificada y el cargador solo enlaza el material cuando la ruta del paquete es de dos tramos; ahora se espera a la imagen y se leen además todas las del paquete por su nombre, con lo que se reconocen el mapa y el UDIM igual que si estuvieran en la carpeta.

### 1.0.0

- **Carga automática de texturas**: al abrir el Hi Poly se buscan los mapas que lo acompañan, también en una subcarpeta, y se agrupan por tipo: Base Color, Normal, Roughness, Metallic, Metallic/Roughness, Height, Ambient Occlusion, Emissive, Opacidad y Specular. Los nombres tipo Substance Painter (`Nombre_BaseColor_1001.png`) identifican material y UDIM, y las texturas incrustadas en GLB, FBX y USDZ se reconocen igual.
- **Selector de mapa**: elige qué textura se ve en los visores y cuál se transfiere al Lo Poly, así que un mismo modelo sirve para generar varias texturas. Los píxeles se envían al motor solo al calcular.
- **Importación de USD**: USDZ y USDA. El USD binario (.usdc) avisa de que hay que exportar a .usda o .usdz.
- **Selector de UDIM en las vistas UV**, que es donde tiene efecto, en lugar de en los visores 3D.
- **Vistas UV a tamaño completo**: ocupan todo el panel y su resolución sigue al tamaño mostrado, de modo que al maximizar se gana área real en vez de ampliar una imagen fija de 512 px.
- **La rueda del ratón** cambia el valor de cualquier desplegable o campo numérico sin desplazar la interfaz.
- Al guardar sin un mapa cargado a mano, el nombre de salida se toma del modelo.
- Pruebas automáticas nuevas: nombres de textura, carga de texturas, bake completo con los mapas detectados e importación de USD.

### 0.3.0

- **Interfaz en rejilla 2×2** que cabe en 1920×1080 sin desplazamiento, con los cuatro visores adaptándose al tamaño de la ventana.
- **Maximizar cada visor** con el icono de su esquina superior derecha o con **Alt+W**; **Escape** restaura la rejilla.
- **Guardar eligiendo carpeta**: diálogo de destino con opción de crear una nueva, memoria de la última carpeta y confirmación antes de reemplazar.
- **Un botón de mapa por UDIM** del Hi Poly, generado automáticamente y marcado en verde cuando ya está cargado. Sustituye al botón único con diálogo de asignación.
- Nueva prueba automática de maquetación dentro de `npm test`.

### 0.2.0

- **Motor de bake reescrito**: BVH sobre el Hi Poly, rejilla de triángulos por tile UV, muestreo de color dentro del kernel y antialiasing de 1 a 8 muestras en una sola pasada.
- **Indicador permanente del motor** en la cabecera: GPU NVIDIA con su modelo, CPU con su número de hilos, o aviso de GPU AMD no aprovechada.
- **Selector de color HSV** con cuadrado de saturación y valor, barra de tono, HEX y RGB.
- **Un solo binario CUDA** con código nativo para sm_75 a sm_120 y PTX de reserva: ya no hace falta una compilación por arquitectura.
- Pruebas que comparan el bake **byte a byte** con una implementación de referencia.

### 0.1.0

- **Primera versión publicada** en este repositorio.
- Transferencia de color del Hi Poly al Lo Poly por proyección sobre las nuevas UV, con motor **CUDA**.
- Carga de **GLB, FBX y OBJ** para los dos modelos, y de la textura de origen en **PNG, JPEG o WebP**.
- **Resolución** de 256 a 8192, **distancia de proyección** configurable y antialiasing de 2, 4 u 8 muestras.
- **Relleno** con margen de 0 a 256 píxeles en sus cinco modos, y color de fondo en RGB y HEX.
- Dos visores 3D con órbita y zoom, dos vistas UV y guardado del PNG junto al mapa de origen.
- Instalador **por usuario**, sin permisos de administrador, con accesos directos en el escritorio y el menú Inicio.

## Licencia

Copyright (c) 2026 Pablo. **Todos los derechos reservados.**

Puedes instalar y usar el programa; no está permitido copiarlo, redistribuirlo, modificarlo ni venderlo sin permiso por escrito del autor. Consulta [LICENSE](LICENSE).

Dentro de la aplicación, el botón **Créditos y licencias**, al pie de la barra lateral, muestra la versión instalada, la autoría y estos mismos avisos.

myRetopoUV usa componentes de terceros con sus propias licencias: Electron (MIT), Chromium y sus librerías (BSD 3-Clause y otras), three.js (MIT), fflate (MIT), NSIS (zlib/libpng, bzip2 y CPL 1.0) y el runtime de NVIDIA CUDA, enlazado estáticamente en el motor de bake (`cudart_static.lib`, redistribuible según el apartado 2.6 del EULA del CUDA Toolkit). Detalles en [THIRD-PARTY-NOTICES.txt](THIRD-PARTY-NOTICES.txt).

Este repositorio solo contiene las descargas; el código fuente no es público.