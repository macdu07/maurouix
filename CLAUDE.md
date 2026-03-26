# MauroUIX — Mini Portfolio

Portfolio personal de Mauricio Correa (MauroUIX), diseñador web y desarrollador UI/UX.

## Stack

- **Framework**: Astro 5 (modo SPA, sin SSR)
- **Estilos**: Tailwind CSS v4 (via `@tailwindcss/vite`)
- **Animaciones**: GSAP + ScrollTrigger (cargado desde CDN en `index.astro`)
- **Lenguaje**: TypeScript (tsconfig estricto)

## Comandos

```bash
npm run dev      # Servidor local en http://localhost:4321
npm run build    # Build de producción → dist/
npm run preview  # Preview del build
```

## Estructura

```
src/
├── components/   # Un componente Astro por sección
│   ├── Header.astro
│   ├── Hero.astro
│   ├── Projects.astro
│   ├── Services.astro
│   ├── Philosophy.astro
│   └── Contact.astro
├── pages/
│   └── index.astro   # Orquesta componentes + toda la lógica GSAP
└── styles/
    └── global.css    # Variables de tema, clases utilitarias custom
```

## Convenciones

- **Idioma del sitio**: español (textos de UI y copy en español)
- **Tema**: oscuro (`#0d0d0d`) con acento cyan (`#00ffd1`)
- **Fuentes**: Inter Tight (display), Figtree (body), JetBrains Mono (mono/etiquetas)
- **Animaciones GSAP**: toda la lógica vive en el `<script>` de `index.astro`; no agregar scripts inline en componentes individuales
- **Estilos custom**: se definen en `global.css` usando `@theme` y `@layer`; preferir clases utilitarias de Tailwind cuando sea posible
- **Sin frameworks de JS**: no agregar React, Vue ni similares; el proyecto es intencionalmente Astro puro

## Identidad / Marca

- **Nombre**: MauroUIX (antes MCD Studio)
- **Email**: contacto@maurouix.com
- **WhatsApp**: +57 302 372 5631
- **Redes**: Instagram @mcorread · LinkedIn /in/mcorread · Facebook /MauricioCorreaD
- **Dominio**: maurouix.co
