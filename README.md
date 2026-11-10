# myRetopoUV

Transferencia de color de un **Hi Poly** a las nuevas UV de un **Lo Poly**, para Windows, de **Pablo**. Carga los dos modelos (**GLB**, **FBX** u **OBJ**) alineados en el mismo espacio 3D y proyecta el **color base** del Hi Poly sobre la malla de retopología. El cálculo se hace por **GPU con CUDA**: resoluciones de 256 a 8192, **antialiasing** de 2, 4 u 8 muestras, **relleno (dilatación)** con margen configurable y soporte de **tiles UDIM**. Incluye dos visores 3D con órbita y zoom y dos vistas UV. Funciona sin conexión.

## Descargar

**[⬇ Descargar la última versión](https://github.com/pabloY/myRetopoUV-descargas/releases/latest)** y, en el apartado *Assets*, elige:

- **`myRetopoUV-Setup-0.1.0-x64.exe`**: instalador (única descarga disponible; no hay versión portable).

**Requisitos:** Windows 10 u 11 de 64 bits y una tarjeta gráfica **NVIDIA** de arquitectura **sm_75** (RTX 20xx, GTX 16xx). No hace falta ser administrador ni tener conexión a internet, ni instalar el CUDA Toolkit: el runtime va dentro del programa y del controlador de NVIDIA solo se usa el que ya tienes instalado.

**Aviso sobre la GPU:** el motor de bake está compilado únicamente para sm_75. En tarjetas de arquitecturas posteriores (RTX 30xx, 40xx) puede no ejecutarse, porque el binario no incluye PTX. Si te ocurre, ponte en contacto e indico cómo generar la versión para tu tarjeta.

## Instalar

1. Descarga el instalador y ábrelo. Se instala **solo para tu usuario** y no pide permisos de administrador: no aparece el aviso de UAC. Copia los archivos a `%LOCALAPPDATA%\Programs\myRetopoUV` y crea accesos directos en el menú Inicio y en el escritorio.
2. **Aviso de Windows.** El instalador no tiene firma digital, así que es normal que:
   - el navegador pregunte si quieres conservar el archivo: elige **Conservar**;
   - Windows muestre «Windows protegió su PC»: pulsa **Más información → Ejecutar de todas formas**.
3. Para desinstalarlo: *Configuración → Aplicaciones → myRetopoUV → Desinstalar*, o el desinstalador que hay en la carpeta del programa.

Si quieres comprobar que la descarga está completa, compara su huella SHA-256 (está en las notas de cada versión) con la que da PowerShell:

```powershell
Get-FileHash .\myRetopoUV-Setup-0.1.0-x64.exe -Algorithm SHA256
```

## Uso rápido

1. Carga el **Hi Poly** y el **Lo Poly** (GLB, FBX u OBJ), alineados en el mismo espacio 3D. Las UV del Lo Poly deben ser únicas dentro de 0–1 o estar repartidas en tiles UDIM.
2. Pulsa **Cargar mapa de color** y elige la textura de origen (PNG, JPEG o WebP).
3. Ajusta **Resolución**, **Distancia de proyección** (en unidades del modelo), **Antialiasing** y el **Relleno / previsualización**:
   - *Sin relleno*: solo los píxeles que reciben proyección.
   - *Dilatación infinita*: extiende el color hasta el borde de la isla.
   - *Dilatación + transparente*, *+ color de fondo* o *+ difusión*: rellenan el margen con transparencia, con el color elegido o difuminando el color de los bordes.
   - **Margen (píxeles)**: ancho del relleno, de 0 a 256. El color de fondo se elige con el cuadro de color, en RGB o HEX.
4. Pulsa **Calcular**. La barra de progreso muestra el avance y **Cancelar** interrumpe el cálculo. El resultado aparece en el visor **Resultado**, junto al **Original**: arrastra para orbitar y usa la rueda para acercar. Las dos vistas de abajo muestran las UV del Hi Poly y del Lo Poly, con navegación por tiles UDIM.
5. **Guardar textura _retopo** escribe el PNG junto al mapa de color que cargaste, con el nombre del mapa y el sufijo `_retopo`, o `_retopo_UDIM` cuando hay varios tiles. Si el archivo ya existe, pide confirmación antes de reemplazarlo.

**Limitaciones conocidas:** el FBX debe ser estático (no se admiten esqueletos), no se leen los MTL ni las texturas externas de un FBX (el color se carga con el botón del mapa) y las caras que cruzan el borde entre dos tiles UDIM se rechazan.

## Historial de versiones

### 0.1.0

- **Primera versión publicada** en este repositorio.
- Transferencia de color del Hi Poly al Lo Poly por proyección sobre las nuevas UV, con motor **CUDA**.
- Carga de **GLB, FBX y OBJ** para los dos modelos, y de la textura de origen en **PNG, JPEG o WebP**.
- **Resolución** de 256 a 8192 y **distancia de proyección** configurable en unidades del modelo.
- **Antialiasing** de 2, 4 u 8 muestras, o sin AA.
- **Relleno** con margen de 0 a 256 píxeles: sin relleno, dilatación infinita, dilatación con transparencia, con color de fondo o por difusión.
- **Color de fondo** con selector en RGB y HEX.
- **Barra de progreso** durante el cálculo y botón de **cancelar**.
- **Dos visores 3D** (original y resultado) con órbita y zoom, y **dos vistas UV** con navegación por tiles.
- **Guardado** del PNG junto al mapa de origen, con sufijo `_retopo` o `_retopo_UDIM`.
- Instalador **por usuario**, sin permisos de administrador, con accesos directos en el escritorio y el menú Inicio.

## Licencia

Copyright (c) 2026 Pablo. **Todos los derechos reservados.**

Puedes instalar y usar el programa; no está permitido copiarlo, redistribuirlo, modificarlo ni venderlo sin permiso por escrito del autor. Consulta [LICENSE](LICENSE).

myRetopoUV usa componentes de terceros con sus propias licencias: Electron (MIT), Chromium y sus librerías (BSD 3-Clause y otras), three.js (MIT), fflate (MIT), NSIS (zlib/libpng, bzip2 y CPL 1.0) y el runtime de NVIDIA CUDA, enlazado estáticamente en el motor de bake (`cudart_static.lib`, redistribuible según el apartado 2.6 del EULA del CUDA Toolkit). Detalles en [THIRD-PARTY-NOTICES.txt](THIRD-PARTY-NOTICES.txt).

Este repositorio solo contiene las descargas; el código fuente no es público.