# myRetopoUV

Transfiere los mapas de un **Hi Poly** a las nuevas UV de un **Lo Poly** en Windows: color base,
normal, rugosidad, metalicidad, altura, oclusión, emisivo u opacidad, con soporte de tiles **UDIM** y
cálculo por **GPU con CUDA**. Incluye retopología con Blender y QuadriFlow, mapeado UV automático y
bake de detalle (Normal, AO y Height). Funciona sin conexión.

## Descargar

**[⬇ Descargar la última versión](https://github.com/pabloY/myRetopoUV-descargas/releases/latest)**
y, en el apartado *Assets*, elige:

- **`myRetopoUV-Setup-<versión>-x64.exe`**: instalador (recomendado).
- `myRetopoUV-<versión>-portable-x64.zip`: versión portable; descomprímela y ejecuta `myRetopoUV.exe`.

### Versiones disponibles

Se conserva la última revisión de cada versión (por ejemplo, la 1.3.1 de la serie 1.3). La versión
portable existe desde la 1.3.

| Versión | Instalador | Portable |
|---|---|---|
| **3.10.1** (última) | [Setup-3.10.1-x64.exe](https://github.com/pabloY/myRetopoUV-descargas/releases/download/v3.10.1/myRetopoUV-Setup-3.10.1-x64.exe) | [3.10.1-portable-x64.zip](https://github.com/pabloY/myRetopoUV-descargas/releases/download/v3.10.1/myRetopoUV-3.10.1-portable-x64.zip) |
| 3.9.0 | [Setup-3.9.0-x64.exe](https://github.com/pabloY/myRetopoUV-descargas/releases/download/v3.9.0/myRetopoUV-Setup-3.9.0-x64.exe) | [3.9.0-portable-x64.zip](https://github.com/pabloY/myRetopoUV-descargas/releases/download/v3.9.0/myRetopoUV-3.9.0-portable-x64.zip) |
| 3.8.0 | [Setup-3.8.0-x64.exe](https://github.com/pabloY/myRetopoUV-descargas/releases/download/v3.8.0/myRetopoUV-Setup-3.8.0-x64.exe) | [3.8.0-portable-x64.zip](https://github.com/pabloY/myRetopoUV-descargas/releases/download/v3.8.0/myRetopoUV-3.8.0-portable-x64.zip) |
| 3.7.0 | [Setup-3.7.0-x64.exe](https://github.com/pabloY/myRetopoUV-descargas/releases/download/v3.7.0/myRetopoUV-Setup-3.7.0-x64.exe) | [3.7.0-portable-x64.zip](https://github.com/pabloY/myRetopoUV-descargas/releases/download/v3.7.0/myRetopoUV-3.7.0-portable-x64.zip) |
| 3.6.0 | [Setup-3.6.0-x64.exe](https://github.com/pabloY/myRetopoUV-descargas/releases/download/v3.6.0/myRetopoUV-Setup-3.6.0-x64.exe) | [3.6.0-portable-x64.zip](https://github.com/pabloY/myRetopoUV-descargas/releases/download/v3.6.0/myRetopoUV-3.6.0-portable-x64.zip) |
| 3.4.1 | [Setup-3.4.1-x64.exe](https://github.com/pabloY/myRetopoUV-descargas/releases/download/v3.4.1/myRetopoUV-Setup-3.4.1-x64.exe) | [3.4.1-portable-x64.zip](https://github.com/pabloY/myRetopoUV-descargas/releases/download/v3.4.1/myRetopoUV-3.4.1-portable-x64.zip) |
| 1.4.0 | [Setup-1.4.0-x64.exe](https://github.com/pabloY/myRetopoUV-descargas/releases/download/v1.4.0/myRetopoUV-Setup-1.4.0-x64.exe) | [1.4.0-portable-x64.zip](https://github.com/pabloY/myRetopoUV-descargas/releases/download/v1.4.0/myRetopoUV-1.4.0-portable-x64.zip) |
| 1.3.1 | [Setup-1.3.1-x64.exe](https://github.com/pabloY/myRetopoUV-descargas/releases/download/v1.3.1/myRetopoUV-Setup-1.3.1-x64.exe) | [1.3.1-portable-x64.zip](https://github.com/pabloY/myRetopoUV-descargas/releases/download/v1.3.1/myRetopoUV-1.3.1-portable-x64.zip) |
| 1.2.1 | [Setup-1.2.1-x64.exe](https://github.com/pabloY/myRetopoUV-descargas/releases/download/v1.2.1/myRetopoUV-Setup-1.2.1-x64.exe) | — |
| 1.1.0 | [Setup-1.1.0-x64.exe](https://github.com/pabloY/myRetopoUV-descargas/releases/download/v1.1.0/myRetopoUV-Setup-1.1.0-x64.exe) | — |
| 1.0.0 | [Setup-1.0.0-x64.exe](https://github.com/pabloY/myRetopoUV-descargas/releases/download/v1.0.0/myRetopoUV-Setup-1.0.0-x64.exe) | — |
| 0.3.0 | [Setup-0.3.0-x64.exe](https://github.com/pabloY/myRetopoUV-descargas/releases/download/v0.3.0/myRetopoUV-Setup-0.3.0-x64.exe) | — |
| 0.2.0 | [Setup-0.2.0-x64.exe](https://github.com/pabloY/myRetopoUV-descargas/releases/download/v0.2.0/myRetopoUV-Setup-0.2.0-x64.exe) | — |
| 0.1.0 | [Setup-0.1.0-x64.exe](https://github.com/pabloY/myRetopoUV-descargas/releases/download/v0.1.0/myRetopoUV-Setup-0.1.0-x64.exe) | — |

**Requisitos:** Windows 10 u 11 de 64 bits. Para acelerar el cálculo, una tarjeta **NVIDIA** GTX 16xx
o cualquier RTX; sin ella funciona igual en la CPU, con el mismo resultado y más tiempo. No hace falta
ser administrador, tener conexión ni instalar el CUDA Toolkit.

## Instalar

1. Abre el instalador. Se instala solo para tu usuario, sin aviso de UAC, y crea accesos directos en
   el menú Inicio y el escritorio.
2. El instalador no tiene firma digital, así que es normal que:
   - el navegador pregunte si quieres conservar el archivo: elige **Conservar**;
   - Windows muestre «Windows protegió su PC»: pulsa **Más información → Ejecutar de todas formas**.

Para desinstalarlo: *Configuración → Aplicaciones → myRetopoUV → Desinstalar*.

Cada versión incluye `SHA256SUMS.txt` con la huella de sus archivos, para comprobar la descarga con
`Get-FileHash <archivo> -Algorithm SHA256` en PowerShell.

## Manuales

- [Cómo nombrar y colocar las texturas del Hi Poly](MANUAL-MAPAS.md)
- [Cómo se guardan los mapas generados y dónde encontrarlos](MANUAL-GUARDADO.md)

## Uso rápido

1. Carga el **Hi Poly** y el **Lo Poly** (GLB, glTF, FBX, OBJ, USDZ, USDA o USDC), alineados en el mismo espacio
   3D, con sus botones o arrastrándolos a los visores. Las texturas del Hi Poly se detectan solas.
2. Ajusta la resolución, la distancia de proyección, el antialiasing y el relleno. Con **Calcular
   todos** (marcado por defecto) se transfieren todos los mapas; desmárcalo para calcular solo el
   elegido en **Mapa de textura**. Los mapas que no se detecten solos se cargan con **Añadir mapa**.
3. Pulsa **Calcular** y, cuando termine, **Guardar**.

Si los modelos no coinciden, **Superponer** los muestra juntos y permite moverlos (`W`), girarlos
(`E`) y escalarlos (`R`). Limitación: el FBX debe ser estático.

## Creado por

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="marca/myApps-logo-oscuro.svg">
  <img src="marca/myApps-logo-claro.svg" alt="myApps" height="56">
</picture>

**myApps** · pabloyt@gmail.com

## Licencia

Copyright (c) 2026 Pablo. **Todos los derechos reservados.** Puedes instalar y usar el programa; no
está permitido copiarlo, redistribuirlo, modificarlo ni venderlo sin permiso por escrito del autor.
Texto completo en [LICENSE](LICENSE); componentes de terceros en
[THIRD-PARTY-NOTICES.txt](THIRD-PARTY-NOTICES.txt).

Este repositorio solo contiene las descargas; el código fuente no es público.
