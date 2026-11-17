# Cómo se guardan los mapas generados y dónde encontrarlos

Guía del botón **Guardar**: qué archivos crea, cómo se llaman y en qué carpeta
quedan según el tipo de LoPoly que hayas cargado o generado. El comportamiento
descrito está implementado en `src/main.js`, `electron/main.cjs` y
`electron/lowpoly.cjs`.

## 1. Qué se guarda

**Guardar** escribe en disco:

- Los mapas obtenidos con **Calcular** (un PNG por tipo de mapa y UDIM).
- Los **bakes de detalle** aceptados desde el panel Bake (Normal, AO o Height).
- La **geometría del LoPoly**, copiada o convertida a FBX.
- Un **manifiesto** que relaciona cada PNG con su tipo y su UDIM.

No se guardan:

- Los mapas de origen del HiPoly.
- El modelo HiPoly.
- Los bakes de detalle que no hayas aceptado.
- Los mapas que nunca hayas calculado.

Si dos resultados producen el mismo tipo de mapa, se guarda uno solo: el
resultado de **Calcular** sustituye al bake de detalle de ese mismo tipo.

## 2. Dónde se guarda

**Guardar** abre un selector de **carpeta**, no un diálogo de «Guardar como».
El formato del modelo no se elige ahí: depende del LoPoly cargado (ver sección 3).

- Si eliges una carpeta llamada `LowPoly` (sin distinguir mayúsculas), los
  archivos se guardan directamente dentro.
- Si eliges cualquier otra carpeta, la aplicación crea dentro una subcarpeta
  `LowPoly`.
- La primera vez propone la carpeta del LoPoly; después recuerda la última
  carpeta usada.

Estructura típica:

```
<Carpeta elegida>/
└─ LowPoly/
   ├─ Silla_low.obj
   ├─ Silla_low.mtl
   ├─ Silla_low.obj.myretopouv.json
   └─ texturas/
      ├─ Silla_low__BaseColor.1001.png
      ├─ Silla_low__NormalGL.1001.png
      └─ Silla_low__Roughness.1001.png
```

Si el LoPoly se ha generado en la aplicación (retopología o UV nuevas), el modelo
se llama `LowPoly.fbx` y las texturas usan ese mismo nombre base:

```
<Carpeta elegida>/
└─ LowPoly/
   ├─ LowPoly.fbx
   ├─ LowPoly.fbx.myretopouv.json
   └─ texturas/
      ├─ LowPoly__BaseColor.1001.png
      └─ LowPoly__NormalDX.1001.png
```

## 3. Tipo del modelo guardado

La aplicación **no convierte entre OBJ, FBX, GLB o USD**. Guarda el LoPoly en el
mismo formato en que lo cargaste. La única conversión es a FBX cuando el LoPoly
se genera dentro de la aplicación.

| LoPoly | Archivo que se guarda | Dependencias que se copian |
|---|---|---|
| **OBJ** | `LowPoly/<nombre>.obj` | El `.mtl` y las texturas que este referencia, conservando rutas relativas |
| **FBX** | `LowPoly/<nombre>.fbx` | Ninguna: solo se copia el FBX |
| **GLB** | `LowPoly/<nombre>.glb` | Ninguna: el GLB es autocontenido |
| **USD / USDA** | `LowPoly/<nombre>.usd` o `.usda` | Los archivos referenciados con `@ruta@` |
| **USDZ** | `LowPoly/<nombre>.usdz` | Ninguna: el USDZ es autocontenido |
| **Generado en la app** (retopología o UV aplicadas) | `LowPoly.fbx` | Ninguna |

Notas:

- La conversión a `LowPoly.fbx` se hace con **Blender**; si no está configurado,
  el guardado de un LoPoly generado no se completa.
- En un FBX cargado no se copian posibles texturas externas: solo el archivo del
  modelo y los PNG calculados.
- En un OBJ, el `.mtl` copiado no se reescribe para apuntar a los PNG nuevos; la
  relación con los mapas generados está en el manifiesto.

## 4. Nombres de las texturas generadas

Todas van a la subcarpeta `texturas/` con este patrón:

```
<Base>__<Etiqueta>.<UDIM>.png
```

- `<Base>` es el nombre del archivo del LoPoly sin extensión. Si el modelo se ha
  generado en la app, es `LowPoly`.
- Hay **dos guiones bajos** entre la base y la etiqueta.
- Hay un **punto** antes del UDIM.
- Siempre son **PNG**.

| Tipo de mapa | Etiqueta en el archivo | Canales en el manifiesto |
|---|---|---|
| Base Color | `BaseColor` | `baseColor: rgb` |
| Normal | `NormalGL` o `NormalDX` | `normal: rgb` |
| Roughness | `Roughness` | `roughness: r` |
| Metallic | `Metallic` | `metallic: r` |
| Metallic/Roughness | `MetallicRoughness` | `roughness: g`, `metallic: b` |
| ORM | `OcclusionRoughnessMetallic` | `ao: r`, `roughness: g`, `metallic: b` |
| RMA | `RMA` | `roughness: r`, `metallic: g`, `ao: b` |
| MRA | `MRA` | `metallic: r`, `roughness: g`, `ao: b` |
| Height | `Height` | `height: r` |
| Ambient Occlusion | `AmbientOcclusion` | `ao: r` |
| Emissive | `Emissive` | `emissive: rgb` |
| Opacity | `Opacity` | `opacity: r` |
| Specular | `Specular` | `specular: rgb` |
| Glossiness | `Glossiness` | `glossiness: r` |
| Independiente | `Independiente` | Sin canales PBR (`{}`) |

La etiqueta `NormalDX` se usa cuando el mapa de origen se reconoció como normal
de DirectX; en caso contrario se usa `NormalGL`.

Los mapas **Independiente** (función pendiente de implementar) se guardan igual
que los demás, con la etiqueta `Independiente`, pero no forman parte de la
composición PBR. En el manifiesto aparecen con `channels: {}`.

Ejemplos:

- LoPoly `Silla_low.obj`, Base Color del UDIM 1001:
  `texturas/Silla_low__BaseColor.1001.png`
- LoPoly `LowPoly.fbx`, Normal DX del UDIM 1002:
  `texturas/LowPoly__NormalDX.1002.png`
- LoPoly `Silla_low.fbx`, Roughness de los UDIM 1001 y 1002:
  `texturas/Silla_low__Roughness.1001.png` y
  `texturas/Silla_low__Roughness.1002.png`

## 5. El manifiesto

Junto al modelo se escribe un archivo:

```
<archivo del modelo>.myretopouv.json
```

Ejemplos: `Silla_low.obj.myretopouv.json`, `LowPoly.fbx.myretopouv.json`.

Contiene:

```json
{
  "version": 1,
  "model": "Silla_low.obj",
  "signature": "<sha256 del modelo>",
  "transform": [ ... 16 números ... ],
  "material": "*",
  "maps": [
    {
      "role": "basecolor",
      "tile": 1001,
      "path": "texturas/Silla_low__BaseColor.1001.png",
      "channels": { "baseColor": "rgb" }
    }
  ]
}
```

Sirve para que myVisor3D y Substance 3D Painter sepan qué PNG corresponde a cada
tipo de mapa y UDIM, y con qué canales.

- Al volver a guardar, el manifiesto **conserva** las entradas de otros tipos y
  UDIM que ya estuvieran registradas.
- Si un mapa de normal cambia de GL a DX (o al contrario), se borra el archivo
  antiguo de la otra convención.
- Si no hay mapas, la transformación es la identidad y no existía un manifiesto
  previo, no se crea el manifiesto: solo se guarda el modelo.

## 6. Sobrescritura y protecciones

- Si ya existe un `LowPoly.fbx` distinto en la carpeta de destino, la aplicación
  pregunta si quieres sustituirlo.
- No permite guardar el FBX generado encima del modelo original: pide otra
  carpeta.
- Si en el destino hay otro modelo con el mismo nombre y contenido distinto,
  muestra un error.
- Si el manifiesto pertenece a otro modelo o su firma no coincide, muestra un
  error.
- Repetir el guardado en la misma carpeta actualiza los PNG y el manifiesto sin
  duplicar archivos.

## 7. Ejemplos completos

### LoPoly OBJ cargado, sin UV nuevas

Entrada: `C:\Trabajo\Silla_low.obj` con su `Silla_low.mtl`.
Destino elegido: `C:\Trabajo`.

```
C:\Trabajo\LowPoly\
├─ Silla_low.obj
├─ Silla_low.mtl
├─ Silla_low.obj.myretopouv.json
└─ texturas\
   ├─ Silla_low__BaseColor.1001.png
   └─ Silla_low__Roughness.1001.png
```

### Retopología generada en la app

Destino elegido: `C:\Trabajo`.

```
C:\Trabajo\LowPoly\
├─ LowPoly.fbx
├─ LowPoly.fbx.myretopouv.json
└─ texturas\
   ├─ LowPoly__BaseColor.1001.png
   └─ LowPoly__NormalGL.1001.png
```

### LoPoly FBX cargado

Entrada: `C:\Trabajo\Silla_low.fbx`.
Destino elegido: `C:\Trabajo`.

```
C:\Trabajo\LowPoly\
├─ Silla_low.fbx
├─ Silla_low.fbx.myretopouv.json
└─ texturas\
   └─ Silla_low__BaseColor.1001.png
```

## 8. Cómo comprobar el resultado

- Al terminar, la barra de estado muestra `Guardado:` seguido de la lista de
  archivos creados.
- Abre la carpeta `LowPoly` para ver el modelo, la carpeta `texturas` y el
  manifiesto.
- Si el guardado se cancela o falla, la barra de estado muestra el motivo; en un
  LoPoly generado, comprueba que Blender esté configurado.
