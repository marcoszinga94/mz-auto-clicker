# Auto-Clicker

> Autoclicker para KDE Plasma 6 en Wayland (Rust + QML), con variantes Python para Linux y Windows.

![Rust](https://img.shields.io/badge/Rust-1.88%2B-000000?logo=rust&logoColor=white)
![Qt/QML](https://img.shields.io/badge/Qt-6%20%2F%20QML-41CD52?logo=qt&logoColor=white)
![KDE Plasma](https://img.shields.io/badge/KDE%20Plasma-6-1D99F3?logo=kde&logoColor=white)
![Python](https://img.shields.io/badge/Python-variantes-3776AB?logo=python&logoColor=white)
![MIT](https://img.shields.io/badge/licencia-MIT-2E8B57)

> **Origen:** Klickmeister es un proyecto basado en [jerry0205/Auto-Clicker](https://github.com/jerry0205/Auto-Clicker), publicado bajo licencia MIT. Este repositorio reúne ese código junto con dos variantes Python para Linux y Windows. La autoría de Klickmeister corresponde a sus contribuidores originales.

## Características

- **Klickmeister (Rust/QML):** clic izquierdo, derecho o central; simple o doble.
- Posición actual del puntero o posición fija en un monitor seleccionado.
- Selector de posición sobre el escritorio, con ajuste fino por flechas.
- Intervalos de 10 ms a 24 h y número fijo de ciclos o clics hasta detener.
- Atajo global configurable (propone `Pause`) mediante el portal de KDE, sin telemetría ni acceso a red.
- **Auto Clicker Simple (Linux, Python):** interfaz GTK4 con botón de activar/desactivar y selector de atajo.
- Inyecta clics reales con un ratón virtual por `/dev/uinput`; incluye autocomprobación.
- Atajo global leyendo los dispositivos en `/dev/input/event*`.
- **Auto Clicker Simple (Windows, Python):** interfaz Tkinter y clics mediante la API `SendInput`.
- Atajo global con el paquete `keyboard`.
- Compilación a `AutoClicker.exe` con PyInstaller y GitHub Actions.

## Stack

| Capa | Tecnología |
| --- | --- |
| Klickmeister | Rust 2024, cxx-qt, Qt 6 / QML, Kirigami |
| Portales Linux | `ashpd` + `xdg-desktop-portal-kde` |
| Variante Linux | Python 3 + GTK4 (PyGObject) |
| Variante Windows | Python 3 + Tkinter + `keyboard` |
| Empaquetado | PKGBUILD, PyInstaller, NSIS/CI |

## Estructura del proyecto

```text
.
├── src/                  # Klickmeister: controlador, scheduler, portal y worker
├── qml/                  # Interfaz Qt/QML (principal, monitores, posición)
├── packaging/            # .desktop, metainfo e iconos
├── PKGBUILD / .SRCINFO   # Paquete para Arch Linux y derivados
├── autoclicker.py        # Variante Python/GTK4 para Linux
├── windows/              # Variante Python/Tkinter para Windows
├── scripts/              # Build del .exe y setup de qmlls
├── tests/                # Pruebas Rust, QML, portal y nativas
├── ARCHITECTURE.md       # Diseño y decisiones técnicas
├── SECURITY_REVIEW.md    # Revisión de seguridad
└── LICENSE
```

## Puesta en marcha

### Requisitos

- **Klickmeister**: KDE Plasma 6, sesión Wayland, portal KDE, Rust 1.88+, Qt 6, Kirigami, `clang` y `lld`.
- **Variante Linux**: GTK4/PyGObject, permiso de escritura en `/dev/uinput` y usuario en el grupo `input`.
- **Variante Windows**: Python 3, `keyboard` y `pyinstaller` para compilar.

### Instalación

Klickmeister en Arch Linux o CachyOS:

```bash
makepkg -si
```

O compilar el checkout actual:

```bash
cargo build --release
```

Variante Python para Linux:

```bash
python3 autoclicker.py
# opcional: instalar en el menú de aplicaciones
./install.sh
```

Para Windows, descargar `AutoClicker.exe` desde las releases o compilar con:

```powershell
powershell -ExecutionPolicy Bypass -File scripts/build-exe.ps1
```

El `AutoClicker.exe` se publica automáticamente en cada tag `v*`.

## Scripts

| Comando | Descripción |
| --- | --- |
| `makepkg -si` | Compila e instala el paquete Arch. |
| `cargo build --release` | Compila Klickmeister desde el checkout. |
| `cargo test --locked --all-targets` | Ejecuta las pruebas Rust. |
| `bash tests/qml-smoke.sh target/release/klickmeister` | Prueba de humo de la interfaz QML. |
| `python3 autoclicker.py --self-test` | Comprueba el ratón virtual en Linux. |
| `./install.sh` | Instala la variante GTK4 en el menú. |
| `scripts/build-exe.ps1` | Compila el ejecutable de Windows. |

## Despliegue

Klickmeister se distribuye como paquete Arch (`PKGBUILD` y `.SRCINFO`) y como binario compilado con Cargo. GitHub Actions ejecuta formato, Clippy, pruebas Rust/portal/QML y validación de escritorio. El workflow `build-exe` adjunta `AutoClicker.exe` a las releases creadas con un tag `v*`.

## Estado del proyecto

Klickmeister va por la versión 0.1.2 y está orientado exclusivamente a KDE Plasma 6 sobre Wayland; X11 no está soportado. Las variantes Python son independientes y cubren Linux y Windows.

## Licencia

MIT. Ver `LICENSE`.

## Autor

Klickmeister: [jerry0205/Auto-Clicker](https://github.com/jerry0205/Auto-Clicker) y sus contribuidores.

Variantes Python y mantenimiento de este repositorio: Marcos Zingaretti — https://github.com/marcoszinga94
