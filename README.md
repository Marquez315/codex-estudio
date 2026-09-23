# Codex Studio

Editor de código en el navegador, inspirado en el estilo de **VSCodium**, con:

- **Editor Monaco** (el mismo motor de VS Code), con resaltado de sintaxis para los lenguajes más comunes.
- **Crear, renombrar y eliminar archivos y carpetas** desde el explorador, igual que en VS Code/VSCodium (íconos arriba del explorador para archivo/carpeta nuevos; al pasar el mouse sobre un archivo aparecen los íconos de renombrar y eliminar).
- **Integración con GitHub**: cargar cualquier repositorio público o privado (con tu token), navegar su árbol de archivos, editar y **publicar commits directamente** (crea, actualiza y elimina archivos reales en el repo).
- **Invitar colaboradores** a un repositorio desde la propia app.
- **Asistente de IA con Groq (100% gratis, sin tarjeta)**: pedile que explique, refactorice o genere código para el archivo abierto, e insertá el resultado con un clic. Se eligió Groq porque expone su API con soporte para llamadas directas desde el navegador (CORS), algo imprescindible acá porque la app no tiene backend propio. (Antes la app ofrecía también OpenRouter y Cerebras como alternativas, pero se sacaron por errores frecuentes de esos proveedores — 401 "User not found" en OpenRouter y 402 "Payment Required" en Cerebras.)
- **Modo offline**: el último repositorio cargado y sus archivos quedan guardados en el navegador (`localStorage`); podés seguir editando sin conexión y publicar los cambios cuando vuelvas a tener internet.
- **Instalable** como app en PC (Windows/Mac/Linux) y en celular (Android/iOS) vía PWA — el botón ⇩ de la barra superior instala la app directamente en Chrome/Edge, o abre el paso a paso manual (Safari/iOS y otros navegadores).
- **Optimizado para escribir código desde el celular**: el editor ajusta el renglón automáticamente, agranda un poco el texto y los botones táctiles, y evita el zoom automático de iOS al tocar un campo de texto.
- **Guía de uso integrada** (botón ❓ en la barra superior): paso a paso para principiantes sobre cómo empezar un proyecto, crear archivos, usar la IA, publicar cambios, trabajar sin conexión e instalar la app.
- 100% estático: **no necesita backend propio**. Todo corre en tu navegador y habla directo con las APIs de GitHub y Groq.

> App creada con fines educativos por **[@codeclubnanduti](https://www.instagram.com/codeclubnanduti/)** y **[@didactica_aumentada3.0](https://www.instagram.com/didactica_aumentada3.0/)** (créditos visibles al pie de la app).

## 1. Probarlo en tu PC ahora mismo

No hace falta instalar nada para probarlo:

```bash
cd codex-studio
python3 -m http.server 8080
```

Abrí `http://localhost:8080` en el navegador. (Abrir `index.html` con doble clic también funciona para probar, pero el Service Worker offline y la instalación como PWA necesitan que se sirva por `http://` o `https://`, no por `file://`.)

## 2. Configurar tus claves

Hacé clic en el ícono de engranaje (⚙) arriba a la derecha y cargá:

1. **Token de acceso personal de GitHub** — creálo en GitHub → *Settings → Developer settings → Personal access tokens → Fine-grained tokens* (o *classic* con el permiso `repo`). Necesario para leer repos privados, hacer commits e invitar colaboradores.
2. **API key de Groq** — necesaria para el panel de asistente de IA. Conseguila gratis (sin tarjeta) en [console.groq.com/keys](https://console.groq.com/keys). Corre modelos abiertos (gpt-oss, Qwen, Kimi) muy rápido y con un cupo diario generoso.

Ninguna de las tres pide tarjeta de crédito para el plan gratuito. Las claves se guardan **solo en tu navegador** (`localStorage`); nunca pasan por ningún servidor intermedio.

## 3. Publicar el proyecto en GitHub (para que quede online)

```bash
cd codex-studio
git init
git add .
git commit -m "Codex Studio: editor web con GitHub e IA"
gh repo create codex-studio --public --source=. --push
# o manualmente: crear el repo en GitHub y luego
# git remote add origin https://github.com/TU-USUARIO/codex-studio.git
# git push -u origin main
```

Después activá **GitHub Pages**: en el repo → *Settings → Pages → Source: rama `main`, carpeta `/ (root)`*. En un par de minutos tu app va a estar disponible en `https://TU-USUARIO.github.io/codex-studio/`, ya instalable en PC y celular.

## 4. Instalar como app

- **Chrome/Edge (PC o Android)**: aparece un botón de instalar (⇩) en la barra superior, o el ícono de instalación en la barra de direcciones.
- **iPhone (Safari)**: botón compartir → *Agregar a pantalla de inicio*.

## Notas y límites a tener en cuenta

- Este proyecto **no incluye ni compila el código fuente de VSCodium** (es una app de escritorio Electron de gran tamaño, con su propio proceso de build); en su lugar, replica la experiencia de edición tipo VS Code en la web usando el mismo motor (**Monaco Editor**) e integra funciones que VSCodium no trae de fábrica: colaboración vía invitaciones de GitHub y un asistente de IA. Si en algún momento querés partir del código real de VSCodium, se puede evaluar incrustar su build web (`code-server`) en lugar de este editor liviano.
- "Invitar colaborador" usa el endpoint de colaboradores de GitHub: la persona recibe una invitación por correo/notificación y debe aceptarla; hace falta que tu token tenga permisos de administrador sobre ese repositorio puntual.
- El modo offline guarda el **último repositorio cargado**. Los cambios hechos sin conexión se marcan como pendientes y se publican con "Guardar y publicar" en cuanto vuelva la conexión.
- Modelo por defecto: `openai/gpt-oss-20b`. Si deja de existir, el asistente muestra un botón para volver al modelo recomendado con un clic.
