# MauroUIX — Portfolio

Portfolio personal de Mauricio Correa, diseñador web y desarrollador UI/UX.

**Sitio:** [maurouix.com](https://maurouix.com)

## Stack

- **Framework:** Astro 5
- **Estilos:** Tailwind CSS v4 (via `@tailwindcss/vite`)
- **Animaciones:** GSAP + ScrollTrigger
- **Lenguaje:** TypeScript
- **Fuentes:** Inter Tight · Figtree · JetBrains Mono

## Estructura

```
src/
├── components/       # Componentes Astro por sección
│   ├── Header.astro
│   ├── Hero.astro
│   ├── Projects.astro
│   ├── Services.astro
│   ├── Philosophy.astro
│   └── Contact.astro
├── data/
│   └── projects.ts   # Datos de proyectos del portfolio
├── pages/
│   ├── index.astro   # Página principal + lógica GSAP
│   └── proyectos/
│       └── [slug].astro  # Páginas de detalle por proyecto
├── styles/
│   └── global.css    # Variables de tema y clases utilitarias
public/
├── favicon.ico
└── favicon.svg
```

## Comandos

| Comando           | Acción                               |
| :---------------- | :----------------------------------- |
| `npm install`     | Instala dependencias                 |
| `npm run dev`     | Servidor local en `localhost:4321`   |
| `npm run build`   | Build de producción en `./dist/`     |
| `npm run preview` | Preview del build antes de desplegar |

## Tema

- Fondo oscuro `#0d0d0d` con acento cyan `#00ffd1`
- Idioma del sitio: español

## Contacto

- **Email:** contacto@maurouix.com
- **WhatsApp:** +57 302 372 5631
- **Instagram:** [@mcorread](https://www.instagram.com/mcorread/)
- **LinkedIn:** [/in/mcorread](https://www.linkedin.com/in/mcorread/)
