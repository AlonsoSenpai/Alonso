# Handoff: Portafolio Alonso Chang (Diseñador Multimedia · Ilustrador)

## Overview
Sitio portafolio personal de una sola página. Primera visita: pantalla de bienvenida → transición de barras → sitio. Dentro, una interfaz tipo "consola mecánica": barra superior, barra lateral de navegación y una ventana central que se reemplaza con animación al cambiar de sección. Fondo: retícula de puntos animada en canvas.

## About the Design Files
`index.html` es una **referencia de diseño hecha en HTML** (prototipo de apariencia y comportamiento), no código de producción. La tarea es **recrearlo** en un stack real. Sin codebase previo, se recomienda **Vite + React (o Astro) + CSS Modules/Tailwind**, con el contenido en archivos de datos (JSON/TS). Para abrir la referencia: servir la carpeta con cualquier servidor estático (`npx serve .`) y abrir `index.html` (necesita `support.js` al lado).

El archivo usa estilos inline y una clase `Component` (lógica, al final del archivo, en `<script type="text/x-dc">`) cuyo método `renderVals()` contiene **todos los datos de contenido** (historia, empleos, proyectos, vídeos, ilustraciones, diseños, dominios). Extraer esos datos tal cual.

## Fidelity
**Alta fidelidad.** Colores, tipografía, formas, animaciones y textos son finales. Recrear pixel-perfect.

## Design Tokens

### Colores (jerarquía dominante / subordinado / secundario / acento)
- Dominante — fondo: `#07080d`; paneles `rgba(11,12,20,.8)` / `rgba(7,8,13,.86)` (levemente transparentes); superficie interna `#0b0c14`, `#05060a`
- Texto: primario `#f5f7ff`, secundario `#dde2ec`, apoyo `#b4bccb` / `#a3abbd`, tenue `#8a93a8`
- Subordinado — marcos metálicos (grafito): `#23262f`, `#0f1117`, `#565d70`, `#8a92a6`, `#3a4050`
- Secundario — **cian**: `#00d9ff` (principal), `#33e1ff`, `#66e8ff`, `#b3f4ff`, `#00b8e6`, `#0099cc`; líneas `rgba(0,217,255,.25–.45)`
- Acento — **magenta**: `#ff2bd6`, `#ff5ce0`, `#ff45da`; solo en CTA, ítem activo, X de cierre, estela del contorno, marcadores
- Transición: barras magenta `#ff2bd6`/`#ff45da`, barra central `#f5f7ff`, texto sobre barras `#07080d`

### Tipografía (Google Fonts)
- **Rajdhani** 500/600/700 — títulos y UI. Títulos de sección: 34–52px (`clamp(34px,5vw,52px)`), 700, uppercase, letter-spacing .06em, text-shadow cromático cian (`3px 0 0 rgba(0,184,230,.9), -2px 0 0 rgba(0,217,255,.55), 0 0 26px rgba(0,217,255,.4)`)
- **Share Tech Mono** — etiquetas, códigos (SEC-01, D-01, I-01), metadatos: 11–14px, letter-spacing .14–.22em, uppercase
- Cuerpo: Rajdhani 17–19px, 500, line-height 1.5–1.55, `text-wrap: pretty`

### Formas
- Sin border-radius. Todo usa **esquinas cortadas** con `clip-path: polygon(...)`: botones 8–14px de chaflán; ventanas grandes 18–23px (octógono).
- Marco de ventana: capa metálica exterior + contorno animado: gradiente cónico que gira (`@property --ang`, `@keyframes spinAng` → 360deg) con estela magenta/cian, recortado al borde con máscara.
- Detalles: reglilla superior (`repeating-linear-gradient` 1px cada 10px), remaches (círculos 7px con gradiente radial), línea de escaneo que baja (`scan` 5s linear infinite).

## Screens / Views

1. **Bienvenida (solo primera visita, `localStorage.hx_visited`)**: ventana central con título "BIENVENIDO", texto "A continuación verás todo el historial de mis trabajos y proyectos." y botón **"Ingresar"**.
2. **Transición**: 9 barras verticales (5 en móvil) bajan con `scaleY` desde el centro hacia afuera (0.6s `cubic-bezier(.77,0,.18,1)`, 55ms de desfase), aparece "ALONSO CHANG" + "Diseñador Multimedia · Ilustrador" con barrido `clip-path`; luego las barras caen hacia abajo revelando el sitio. Total ~3.1s.
3. **Sitio**: grid barra superior (nombre "Alonso Chang | Diseñador Multimedia | Ilustrador", altura fija) + barra lateral (menú de 8 secciones con código 01–08) + ventana principal. En móvil (<820px) el menú pasa a una fila horizontal desplazable; <560px ajustes compactos.

### Secciones
- **01 Historia** — biografía + línea de tiempo (1995 → 2018). Foto `img/alonso.jpg`.
- **02 Empleos** — lista de cargos por año (2012–2025).
- **03 Proyectos** — tarjetas 16:10: Starclans, Neon Tetris (`img/neon-tetris.png`), Medidor de porciones (captura vía `image.thum.io` + botón "Visitar sitio ›" → medidordeporciones.com), Koaster.cl (idem → koaster.cl). **Recomendación:** reemplazar thum.io por capturas estáticas propias.
- **04 Vídeos** — miniatura YouTube (`J6aJD-9avko`) que al pulsar se convierte en iframe embebido.
- **05 Ilustraciones** — grilla de cuadrados 1:1 (`object-fit: cover`), 50 imágenes desde Google Drive (`https://lh3.googleusercontent.com/d/<ID>=w800`; IDs en la constante `ILLUS`). **Lightbox**: ventana con el marco del sitio que entra desde la derecha (`slideInR` .55s), la imagen carga con `glitchIn`; flechas ←/→, clic en mitad izquierda/derecha, X y Esc para cerrar; contador "3 / 50"; bucle.
- **06 Diseños** — tarjetas 1:1 (Fumix 2023, Mihassa 2022, Bonbonss, Blox 2026, FyG Gasfitería). Cada una tiene reglas de miniatura propias (flags en `DESIGNS`: `clear` fondo transparente, `full` alto completo, `fitW` ancho completo, `diag` dos imágenes en diagonal, `thumbImgs` override). Al pulsar: **ventana de detalle** que sube desde abajo (`slideUpIn` .6s) con: cabecera (código · tipo · título), imagen principal, tabla de datos (Cliente/Año/Rol/Herramientas), "01 · Encargo", "02 · Galería/Variantes". X, Esc o clic fuera → baja (`slideDownOut` .5s).
- **07 Dominio** — la ventana principal se oculta; solo queda una **ventana pequeña** (max 560px) con título y 3 botones: Máquinas, Software, Inteligencia artificial. Cada botón abre un **panel lateral derecho** (520px, sin overlay oscuro) que entra desde la derecha con barras de porcentaje segmentadas cian (crecen con `growW` .9s escalonado) y marcador magenta. Elegir otro dominio: el panel sale y entra el nuevo. Porcentajes en `DOMAINS` (estimados; confirmar con el cliente).
- **08 Contacto** — texto de invitación + formulario (Nombre, Correo, Asunto, Mensaje). Envío por `fetch` POST a la constante `FORM_ENDPOINT` (FormSubmit AJAX). Incluye campo trampa `_honey` (antispam), `_replyto` con el correo del visitante (responder directo desde Gmail) y asunto "Portafolio · …". Tras confirmar el primer correo de activación de FormSubmit, reemplazar el correo del endpoint por el alias aleatorio que entrega FormSubmit. Estados: enviando ("Transmitiendo…"), éxito ("TRANSMISIÓN COMPLETADA"), error.

## Interactions & Behavior
- **Cambio de sección**: la ventana central sale por la **derecha** y la nueva entra **desde la derecha** (siempre). Al cambiar se cierran todas las ventanas emergentes abiertas.
- **Texto que se tipea**: al aparecer por primera vez, los textos se escriben rápido con cursor "▌"; se detiene y completa al cambiar de sección.
- **Entrada de imágenes**: `glitchIn` (~.95s, clip-path por franjas + brillo) + ruido (`noise`) + línea de corte (`tear`), escalonado ~90ms por ítem.
- **Scrollbar propio** en la ventana central: riel a la derecha con marcas, pulgar magenta con puntas de rombo, porcentaje 000–100 arriba; arrastrable y clicable. Scrollbar nativa oculta.
- **Fondo**: canvas con retícula de puntos cada 30px, onda seno que modula tamaño/opacidad; cerca del cursor los puntos se vuelven magenta, se apartan y se conectan con líneas.
- Hover: botones aclaran (`brightness(1.12)`) o desplazan 3–4px; bordes pasan a magenta.

## State Management
`phase` (intro/site), `tPhase` (idle/in/out), `active` (sección), `busy` (animación en curso), `booted`, `lb` (índice lightbox), `dz` + `dzOut` (detalle de diseño), `dm` + `dmOut` (panel de dominio), `form` (null/sending/sent/error), `playing` (vídeo), `narrow`/`mobile` (breakpoints 820/560px), `sc` (scroll: progreso, tamaño).

## Assets
- `img/alonso.jpg` — foto de perfil (Historia)
- `img/neon-tetris.png` — captura mejorada de Neon Tetris
- Ilustraciones y diseños: Google Drive público del autor (IDs en el código), enlaces permanentes — se mantienen tal cual.

## SEO / Metadatos
- `<title>`: **Alonso Chang - Diseñador**
- `meta description`, Open Graph (`og:title`, `og:description`, `og:image` = `img/alonso.jpg`, `og:locale` es_CL) y `theme-color` `#07080d`. Dominio: alonsochang.cl (archivo `CNAME`, DNS en Cloudflare → GitHub Pages).
- Vídeo: YouTube `J6aJD-9avko`

## Pendientes de contenido
Años de Bonbonss y FyG Gasfitería; rol/herramientas confirmados de cada diseño; porcentajes reales de Dominio; enlace jugable de Neon Tetris.

## Files
- `index.html` — prototipo completo (plantilla + lógica + datos)
- `support.js` — runtime necesario solo para ver el prototipo
- `img/` — imágenes locales
