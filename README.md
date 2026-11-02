# Bibliotheca

> Aplicación lectora y gestora de biblioteca multiplataforma, liviana y offline-first. Inspirada en la filosofía minimalista de *Adhan Companion*: app-like, sin bloat, sin fricciones y con datos 100% bajo control del usuario.

---

## 📖 Concepto

Bibliotheca combina en una sola aplicación un **visor de lectura fluido (PDF y EPUB)** y un **gestor de biblioteca local**, diseñado para arrancar en segundos y consumir una fracción de la memoria y CPU de visores pesados basados en Electron.

- **100% Offline-first**: Tus libros, portadas, marcadores y notas viven localmente en una base de datos **SQLite**.
- **Lectura y anotaciones**: Resaltado de texto, marcadores y notas al margen sin tocar ni alterar tus archivos originales.
- **Continuidad automática**: Retoma de inmediato en la última página o capítulo leído (*«Seguir leyendo»*).
- **Extensible y conectable**: Sincronización básica mediante WebDAV (Fase 1) y arquitectura lista para conectar con un servidor propio dedicado (Fase 2) con sync en tiempo real vía WebSockets, descarga bajo demanda y versión Web.

---

## 🛠️ Stack Tecnológico

| Capa | Tecnología |
|---|---|
| **Framework** | [Tauri v2](https://v2.tauri.app/) (Rust + WebView nativo) |
| **Frontend** | [React 19](https://react.dev/) + [TypeScript](https://www.typescriptlang.org/) + [Vite](https://vitejs.dev/) |
| **Estilos** | [Tailwind CSS](https://tailwindcss.com/) |
| **Persistencia** | [SQLite](https://www.sqlite.org/) |
| **Motores** | [pdf.js](https://mozilla.github.io/pdf.js/) + [epub.js](https://github.com/futurepress/epub.js/) |

---

## 🚀 Desarrollo

### Requisitos

- **Node.js**: >= 20
- **pnpm**: >= 9
- **Rust toolchain**: `rustc` y `cargo` (edición 2024 / 1.85+)
- **Dependencias Linux**: `libwebkit2gtk-4.1-dev libgtk-3-dev pkg-config`

### Comandos

```bash
# Instalar dependencias
pnpm install

# Modo desarrollo (inicia Vite y abre la ventana Tauri con hot-reload)
pnpm tauri dev

# Compilar solo el frontend
pnpm build

# Compilar solo el core Rust
cd src-tauri && cargo check

# Generar paquete para distribución (.deb)
pnpm tauri build
```

---

## 📄 Licencia

Este proyecto está bajo la Licencia MIT. Ver el archivo [LICENSE](LICENSE) para más detalles.
