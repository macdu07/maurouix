# AGENTS.md — MauroUIX Portfolio

Guía para agentes de IA que trabajen en este repositorio.

## Contexto del proyecto

Mini portfolio de una sola página para Mauricio Correa (MauroUIX).
Stack: **Astro 5 + Tailwind CSS v4 + GSAP**.
Sin backend, sin CMS, sin frameworks de componentes reactivos.

## Reglas clave

### Lo que SÍ hacer

- Mantener el español en todos los textos visibles del sitio.
- Respetar el sistema de diseño claro: fondo marfil `#F4EFE6`, acento principal verde esmeralda profundo `#063F35`, detalle dorado suave `#C8A45D`, negro suave `#171717` y tipografías definidas en `global.css`.
- Agregar animaciones de interfaz con GSAP. Mantener el scroll físico nativo para evitar sensación de lag; los anchors pueden usar suavidad programática ligera.
- Usar clases de Tailwind v4 para estilos; agregar clases custom en `src/styles/global.css` si son reutilizables.
- Mantener cada sección en su propio componente (`src/components/`).

### Lo que NO hacer

- No instalar frameworks JS (React, Vue, Svelte, etc.).
- No agregar SSR ni adaptadores de Astro; el sitio es estático.
- No crear estilos inline en componentes si ya existe una clase en `global.css`.
- No cambiar la paleta de colores ni las fuentes sin confirmación del usuario.
- No dividir `index.astro` en múltiples páginas; es un one-pager intencional.

## Flujo de trabajo sugerido

1. `npm run dev` para desarrollo local.
2. Editar componentes en `src/components/` o estilos en `src/styles/global.css`.
3. Para animaciones nuevas, agregar el bloque GSAP en el `<script>` de `index.astro`.
4. Verificar con `npm run build` antes de considerar el trabajo terminado.

## Información de contacto (para no inventar datos)

- Email: soporte@maurouix.com
- WhatsApp: +57 302 372 5631
- Instagram: @maurouix
- LinkedIn: /in/mcorread
- Facebook: /MauricioCorreaD
- Dominio: maurouix.co
