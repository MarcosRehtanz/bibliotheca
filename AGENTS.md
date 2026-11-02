# AGENTS.md

## Qué es este repo

Aplicación lectora y gestora de biblioteca multiplataforma, liviana y offline-first: **"Bibliotheca"** — visor de PDF y EPUB, gestión de colecciones, marcadores, resaltados con notas, y sincronización opcional (WebDAV en Fase 1 y servidor propio dedicado en Fase 2). Construida con **Tauri v2** (Rust + React/TypeScript).

El **spec de producto y arquitectura vive fuera del repo**, en el vault Obsidian (en el pendrive SanDisk o disco local):
- Principal (pendrive): `/run/media/usuario/MARCOSUSB/Obsidian/Ingenieria/Proyectos/Bibliotheca/`
- Fallback (copia en disco): `/home/usuario/Documentos/Obsidian/Ingeniería/Proyectos/Bibliotheca/`
Consultar siempre el spec antes de decidir arquitectura, contratos IPC, esquema de base de datos o flujos de usuario:
- `00 - Índice.md` — Visión global y enlaces
- `01 - Planificación.md` — Objetivos, MVP, stack técnico y arquitectura de expansión
- `02 - Decisiones Técnicas.md` — Tauri v2 vs Electron, SQLite local, motores de lectura y desacoplamiento de datos
- `02 Frameworks/Desktop/Tauri.md` (en el mismo vault) — Gotchas técnicos de Tauri en Linux/Wayland

## Estructura (Tauri 2 + React/TS)

- `src-tauri/` — **Core nativo en Rust**: comandos Tauri, acceso a filesystem, base local SQLite (`sqlx`), parsers y motor de sincronización.
  - `src-tauri/src/lib.rs` (`run()`) — Punto de entrada real de la aplicación Tauri.
  - `src-tauri/src/main.rs` — Delegado de ejecución.
  - `src-tauri/capabilities/` — Permisos y declaraciones de seguridad frontend→core. En Tauri 2 todo comando accesible por el frontend debe estar declarado aquí.
- `src/` — **Frontend React 19 + TypeScript**:
  - Interfaz construida con Tailwind CSS y componentes accesibles.
  - Gestión de estado con Zustand desacoplada mediante interfaz de repositorio (`LibraryRepository`).
  - Visores: integración de `pdf.js` y `epub.js`.

## Comandos

```bash
# Desarrollo: levanta Vite en :1420 + la ventana nativa Tauri con hot-reload
pnpm tauri dev

# Compilar solo el core Rust
cd src-tauri && cargo check

# Validar TypeScript y compilar el frontend
pnpm build

# Build de producción para Linux (.deb)
pnpm tauri build
```

⚠️ En shells no interactivos o entornos sandbox, asegurar que `cargo` esté en PATH:
`export PATH="$HOME/.cargo/bin:$PATH"` o `source ~/.cargo/env`.

## Gotchas Linux verificados en esta máquina (Ubuntu 26.04 / GNOME 50 / WebKitGTK 2.52)

- WebKitGTK 2.52.6 en Wayland corre de manera nativa sin el bug histórico de DMABUF.
- Dependencias del sistema ya instaladas: `libwebkit2gtk-4.1-dev libgtk-3-dev libayatana-appindicator3-dev librsvg2-dev pkg-config libasound2-dev`.

## Reglas de producto y arquitectura

1. **Offline-first estricto**: La app debe funcionar al 100% sin conexión a internet. Los libros, las carátulas cacheadas, el progreso y las notas se almacenan de manera local en SQLite.
2. **Desacoplamiento de datos**: La UI nunca debe acoplarse rígidamente a Tauri Commands directos. Todas las llamadas pasan por una capa de abstracción (`LibraryRepository`) que permitirá cambiar a la API del servidor propio en la Fase 2 sin alterar los componentes visuales.
3. **Manejo explícito de estados**: Cada pantalla o componente debe contemplar sus estados: cargando, vacío, disponible, leyendo, error y sincronizando. No asumir éxito antes de que el core lo confirme.
4. **Minimalismo y velocidad**: Arranque en menos de 2 segundos, bajo consumo de RAM (<60 MB inicial), sin distracciones en el modo de lectura.
