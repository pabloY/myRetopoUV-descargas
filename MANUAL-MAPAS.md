# Cómo nombrar y colocar las texturas del HiPoly

Guía de la detección automática de texturas al abrir un modelo HiPoly. Las reglas
descritas aquí están implementadas en `electron/patterns.cjs` y `electron/main.cjs`:
son las que usa la aplicación para reconocer el tipo de mapa, el UDIM y el conjunto
de texturas de cada archivo.

## 1. Dónde deben guardarse

- En la **misma carpeta** que el archivo del HiPoly.
- También se buscan en las **subcarpetas inmediatas** (un solo nivel):

  ```
  MiModelo/MiModelo_BaseColor_1001.png              ✔
  MiModelo/texturas/MiModelo_BaseColor_1001.png     ✔
  MiModelo/texturas/4k/MiModelo_BaseColor_1001.png  ✘ (dos niveles)
  ```

- Se escanean como máximo **600 imágenes**.
- Formatos admitidos: **PNG, JPG/JPEG y WebP**. No se leen TGA, TIFF, EXR ni BMP.
- El HiPoly debe tener **UVs**; las texturas se asignan a las tiles UDIM del modelo.

## 2. Regla del nombre

Formato recomendado:

```
<Conjunto>_<Tipo>_<UDIM>.<ext>
```

También se acepta el UDIM en medio o el tipo al principio:

```
<Conjunto>_<UDIM>_<Tipo>.<ext>
<Tipo>_<Conjunto>_<UDIM>.<ext>
```

- Los separadores pueden ser `_`, `-`, `.` o espacios; cualquier carácter que no
  sea letra o número separa palabras.
- Mayúsculas y minúsculas dan igual.
- **El UDIM debe ir separado** del resto por un separador. `MiModelo_BaseColor1001.png`
  no se reconoce.

## 3. Tipos de mapa aceptados

El tipo debe ser la **primera o la última palabra** del nombre, descontando el UDIM
y la resolución.

| Tipo | Alias aceptados | Uso / canales |
|---|---|---|
| **Base Color** | `basecolor`, `basecolour`, `albedo`, `diffuse`, `difusa`, `difuso`, `color`, `colour`, `base` | Color RGB |
| **Normal** | `normalgl`, `normaldx`, `normalmap`, `normals`, `normal`, `nrm` | Normal en espacio tangente |
| **Roughness** | `roughness`, `rough`, `rugosidad`, `rgh` | Canal R |
| **Metallic** | `metalness`, `metallic`, `metalico`, `metal`, `mtl` | Canal R |
| **Metallic/Roughness** | `metallicroughness`, `metalnessroughness` | G = rugosidad, B = metalicidad |
| **ORM** | `occlusionroughnessmetallic`, `orm` | R = AO, G = rugosidad, B = metalicidad |
| **RMA** | `rma` | R = rugosidad, G = metalicidad, B = AO |
| **MRA** | `mra` | R = metalicidad, G = rugosidad, B = AO |
| **Height** | `displacement`, `heightmap`, `height`, `desplazamiento`, `altura`, `disp` | Escala de grises |
| **Ambient Occlusion** | `ambientocclusion`, `occlusionmap`, `occlusion`, `ambient`, `ao` | Canal R |
| **Emissive** | `emissive`, `emission`, `emision`, `emit` | Color RGB |
| **Opacity** | `opacity`, `transparency`, `transparent`, `alpha`, `mask` | Canal R |
| **Specular** | `specular`, `spec` | Color RGB |
| **Glossiness** | `glossiness`, `gloss` | Canal R; el motor lo invierte a rugosidad |

### Normales GL y DX

Si la normal es de DirectX, el nombre debe contener `NormalDX` (o `normal_dx`,
`normal-dx`, `normal.dx`, `directx`). Así el motor invierte la componente Y al
calcular y al previsualizar. Si es OpenGL, usa `NormalGL` o `Normal`.

## 4. El «conjunto» y por qué importa

El conjunto es la parte del nombre que queda después de quitar el tipo, el UDIM y
la resolución. La aplicación lo compara con el **nombre del HiPoly sin extensión**:

1. Coincidencia exacta: prioridad máxima.
2. El conjunto empieza por el nombre del modelo, o el nombre del modelo empieza por
   el conjunto: siguiente prioridad.
3. El conjunto contiene el nombre del modelo, o al contrario: siguiente.

**Solo se carga automáticamente el mejor conjunto.** Si en la carpeta hay texturas
de varios modelos, nombra cada conjunto igual que su modelo:

```
Silla.fbx
Silla_BaseColor_1001.png
Silla_Normal_1001.png
Mesa_BaseColor_1001.png      ← no se carga al abrir Silla.fbx
```

Usa exactamente el nombre del HiPoly como conjunto. Si el modelo se llama
`Silla_01.fbx`, el conjunto debe ser `Silla_01`.

Si el nombre no tiene conjunto (por ejemplo `BaseColor_1001.png`), el conjunto
queda como `texture`; si es el único conjunto de la carpeta, se carga igualmente.

## 5. UDIM

- Cuatro dígitos, entre **1001 y 1999**.
- Se usa el **primer** UDIM que aparezca en el nombre; un archivo por UDIM.
- Si el nombre **no lleva UDIM**, la textura se aplica a **todas** las tiles del
  modelo.
- El UDIM debe existir en el HiPoly; si el modelo no tiene esa tile, la textura no
  se usa.
- Un token de resolución al final se ignora y no forma parte del conjunto:
  `256`, `512`, `1024`, `2048`, `4096`, `8192`, `1k`, `2k`, `4k`, `8k`, `16k`.

## 6. Ejemplos que funcionan

| Archivo | Tipo | UDIM | Conjunto |
|---|---|---|---|
| `Silla_BaseColor_1001.png` | Base Color | 1001 | silla |
| `Silla_1001_BaseColor.png` | Base Color | 1001 | silla |
| `Silla_BaseColor.1001.png` | Base Color | 1001 | silla |
| `Silla_Base Color_1001.png` | Base Color | 1001 | silla |
| `Silla_Color_1001.jpg` | Base Color | 1001 | silla |
| `Silla_NormalGL_1001.png` | Normal GL | 1001 | silla |
| `Silla_NormalDX_1001.png` | Normal DX | 1001 | silla |
| `Silla_Roughness_1002.png` | Roughness | 1002 | silla |
| `Silla_MetallicRoughness_1001.png` | Metallic/Roughness | 1001 | silla |
| `Silla_ORM_1001.png` | ORM | 1001 | silla |
| `Silla_RMA_1001.png` | RMA | 1001 | silla |
| `Silla_MRA_1001.png` | MRA | 1001 | silla |
| `Silla_AmbientOcclusion_1001.png` | Ambient Occlusion | 1001 | silla |
| `Silla_Height_1001.png` | Height | 1001 | silla |
| `Silla_Emissive_1001.png` | Emissive | 1001 | silla |
| `Silla_Opacity_1001.png` | Opacity | 1001 | silla |
| `Silla_Specular_1001.png` | Specular | 1001 | silla |
| `Silla_BaseColor_4k.png` | Base Color | todas | silla |
| `Silla_BaseColor_1001_2048.png` | Base Color | 1001 | silla |
| `BaseColor_Silla_1001.png` | Base Color | 1001 | silla |

## 7. Ejemplos que NO funcionan

| Archivo | Motivo |
|---|---|
| `Silla_BaseColorFinal_1001.png` | El tipo no está al principio ni al final; sobra `Final`. |
| `Silla_Metálico_1001.png` | El acento se elimina y queda `metlico`, que no coincide con ningún alias. |
| `Silla_BaseColor_1.png` | El UDIM debe tener cuatro dígitos. |
| `Silla_BaseColor_2001.png` | Fuera del rango 1001–1999. |
| `SillaBaseColor1001.png` | El UDIM no está separado del tipo. |
| `Silla_BaseColor_1001.tga` | Formato no admitido. |
| `Silla_BaseColor_1001_extra.png` | El tipo queda en el medio; no se reconoce. |

**Evita acentos y eñes** en el tipo de mapa: no se transliteran, se eliminan.

## 8. Texturas incrustadas en el modelo

En GLB, FBX y USDZ no hacen falta archivos aparte:

- Si el material del modelo ya referencia la textura (por ejemplo, en el slot de
  color base, normal o rugosidad), se carga aunque el nombre interno no siga el
  patrón.
- Si la textura incrustada sigue el patrón de nombre, también se le asigna su UDIM
  como si estuviera en la carpeta.

## 9. Cómo comprobar que se han detectado

1. Al abrir el HiPoly, el selector **Mapa de textura** muestra cada tipo detectado
   y cuántos UDIM tiene: `Base Color · 2 UDIM`, `Normal · 2 UDIM`…
2. Los botones **Cargar mapa 1001**, **Cargar mapa 1002**… se ponen en verde con
   `✓` cuando ese UDIM ya tiene un mapa del tipo seleccionado.
3. Debajo del selector aparece el nombre de los archivos cargados.
4. Si un tipo no aparece, revisa en este orden: formato, separadores, UDIM de
   cuatro dígitos, alias del tipo y nombre del conjunto.

## 10. Mapas que no siguen el patrón: «Añadir mapa»

Los mapas cuyo nombre no encaja no se cargan automáticamente. El botón
**Añadir mapa**, situado bajo el selector **Mapa de textura**, permite
importarlos a mano.

### 10.1 Cómo se usa

1. Abre un HiPoly con UV.
2. Pulsa **Añadir mapa**. El selector de archivos se abre directamente en la
   carpeta del HiPoly.
3. Selecciona una o varias imágenes (PNG, JPG/JPEG, WebP).
4. Revisa la lista: cada archivo muestra el UDIM detectado o «Todos los UDIM»
   y una lista de botones de opción con el **tipo de mapa**, en tres grupos:
   Color base, Metallic, Roughness, Emisivo y Specular; Normal, Height / Bump y
   Ambient occlusion; Opacidad, Transmission, Translucency, SSS, Thickness e IOR.
   Los empaquetados (Metallic/Roughness, ORM, RMA, MRA), Glossiness e
   **Independiente** están plegados en **Más tipos**. El tipo detectado por el
   nombre llega marcado. Si el nombre no lo indica, ninguno está marcado, se
   avisa en amarillo y **Añadir** no se activa hasta que lo elijas. Debajo de la
   lista se explica la función del tipo elegido en el material PBR.
   Con un empaquetado elegido aparece la casilla **Separar en mapas
   independientes**. Si la marcas, se cargan cuatro mapas: el empaquetado y uno en
   escala de grises por canal (Ambient Occlusion, Roughness y Metallic; el de
   Metallic/Roughness solo tiene los dos últimos). Los separados se guardan como
   PNG en la carpeta del original, con su nombre y el tipo del canal
   (`Foo_ORM_1001.png` → `Foo_AmbientOcclusion_1001.png`, `Foo_Roughness_1001.png`
   y `Foo_Metallic_1001.png`), y la próxima vez se reconocen solos. Si ya existe
   un archivo con ese nombre, no se sustituye y se avisa.
5. **Cambiar archivos** vuelve a abrir el selector. Pulsa **Añadir**.

### 10.2 Asignación

- Si el nombre contiene un UDIM de cuatro dígitos entre 1001 y 1999, se asigna a
  esa tile.
- Si no contiene UDIM, se aplica a todas las tiles del HiPoly.
- Si el UDIM no existe en el HiPoly, el archivo se omite y se avisa.
- El tipo que queda elegido en la lista manda, aunque el nombre indique otro.
- Si ya había mapas de ese tipo en esas tiles, se sustituyen por los nuevos.
- No se aplica la selección de conjunto de la búsqueda automática: los archivos se
  añaden al HiPoly abierto, sin comparar su nombre con el del modelo.

### 10.3 Tipo «Independiente»

- **No forma parte de la composición PBR**: pulsar PBR no lo usa ni lo mezcla con
  los demás mapas.
- Se muestra al seleccionarlo en **Mapa de textura**, como cualquier otro mapa.
- Se puede calcular con **Calcular** y guardar con **Guardar**.
- En el guardado se llama `<Base>__Independiente.<UDIM>.png` y no lleva canales
  PBR en el manifiesto.
- No participa en **Sumar bake**.
