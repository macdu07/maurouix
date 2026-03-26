# Plan de Acción SEO -- maurouix.com

**Score actual: 32/100 | Score estimado post-implementación: 70-80/100**

---

## CRITICAL -- Corregir inmediatamente

### 1. Crear robots.txt
**Archivo:** `public/robots.txt`
**Tiempo:** 30 min

```
User-agent: *
Allow: /

User-agent: GPTBot
Allow: /

User-agent: OAI-SearchBot
Allow: /

User-agent: ClaudeBot
Allow: /

User-agent: PerplexityBot
Allow: /

User-agent: CCBot
Disallow: /

User-agent: cohere-ai
Disallow: /

Sitemap: https://maurouix.com/sitemap-index.xml
```

---

### 2. Instalar sitemap
**Archivos:** `astro.config.mjs`, `package.json`
**Tiempo:** 30 min

```bash
npm install @astrojs/sitemap
```

```js
// astro.config.mjs
import sitemap from '@astrojs/sitemap';

export default defineConfig({
  site: 'https://maurouix.com',
  integrations: [sitemap()],
  vite: { plugins: [tailwindcss()] }
});
```

---

### 3. Agregar etiquetas canonical
**Archivos:** `index.astro`, `[slug].astro`
**Tiempo:** 15 min

Agregar en el `<head>` de cada página:
```html
<link rel="canonical" href="https://maurouix.com/" />
```

En `[slug].astro`:
```html
<link rel="canonical" href={`https://maurouix.com/proyectos/${project.slug}`} />
```

---

### 4. Agregar Open Graph y Twitter Cards
**Archivos:** `index.astro`, `[slug].astro`
**Tiempo:** 1 hora

Homepage:
```html
<meta property="og:title" content="MauroUIX — Diseñador Web & Desarrollador UI/UX" />
<meta property="og:description" content="Portfolio de Mauricio Correa. +10 años diseñando experiencias digitales para empresas en Colombia." />
<meta property="og:image" content="https://maurouix.com/og-image.jpg" />
<meta property="og:url" content="https://maurouix.com/" />
<meta property="og:type" content="website" />
<meta property="og:locale" content="es_CO" />
<meta name="twitter:card" content="summary_large_image" />
<meta name="twitter:title" content="MauroUIX — Diseñador Web & Desarrollador UI/UX" />
<meta name="twitter:description" content="Portfolio de Mauricio Correa. +10 años diseñando experiencias digitales." />
<meta name="twitter:image" content="https://maurouix.com/og-image.jpg" />
```

Proyecto (dinámico):
```html
<meta property="og:title" content={`${project.title} — MauroUIX`} />
<meta property="og:description" content={project.description} />
<meta property="og:url" content={`https://maurouix.com/proyectos/${project.slug}`} />
<meta property="og:type" content="article" />
```

**Nota:** Necesitas crear una imagen OG de 1200x630px.

---

### 5. Agregar JSON-LD (Datos Estructurados)
**Archivos:** `index.astro`, `[slug].astro`
**Tiempo:** 2 horas

**Homepage** -- agregar en `<head>`:
```html
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@graph": [
    {
      "@type": "WebSite",
      "@id": "https://maurouix.com/#website",
      "url": "https://maurouix.com",
      "name": "MauroUIX",
      "description": "Diseñador Web & Desarrollador UI/UX",
      "inLanguage": "es",
      "publisher": { "@id": "https://maurouix.com/#person" }
    },
    {
      "@type": "Person",
      "@id": "https://maurouix.com/#person",
      "name": "Mauricio Correa",
      "alternateName": "MauroUIX",
      "url": "https://maurouix.com",
      "jobTitle": "Diseñador Web & Desarrollador UI/UX",
      "description": "Diseñador web y desarrollador UI/UX con más de 10 años de experiencia.",
      "knowsAbout": ["UI/UX Design", "WordPress", "Bricks Builder", "Figma", "Identidad Corporativa"],
      "email": "mailto:contacto@maurouix.com",
      "telephone": "+573023725631",
      "address": { "@type": "PostalAddress", "addressCountry": "CO" },
      "sameAs": [
        "https://www.instagram.com/mcorread",
        "https://www.linkedin.com/in/mcorread",
        "https://www.facebook.com/MauricioCorreaD"
      ]
    },
    {
      "@type": "ProfessionalService",
      "@id": "https://maurouix.com/#service",
      "name": "MauroUIX",
      "url": "https://maurouix.com",
      "provider": { "@id": "https://maurouix.com/#person" },
      "areaServed": { "@type": "Country", "name": "Colombia" },
      "serviceType": [
        "Diseño de Interfaces Web (UI/UX)",
        "Desarrollo de Sitios Web Creativos",
        "Conceptualización de Imagen Corporativa"
      ],
      "telephone": "+573023725631",
      "email": "contacto@maurouix.com"
    }
  ]
}
</script>
```

**Páginas de proyecto** -- agregar en `<head>` de `[slug].astro`:
```astro
<script type="application/ld+json" set:html={JSON.stringify({
  "@context": "https://schema.org",
  "@graph": [
    {
      "@type": "BreadcrumbList",
      "itemListElement": [
        { "@type": "ListItem", "position": 1, "name": "Inicio", "item": "https://maurouix.com" },
        { "@type": "ListItem", "position": 2, "name": "Proyectos", "item": "https://maurouix.com/#work" },
        { "@type": "ListItem", "position": 3, "name": project.title }
      ]
    },
    {
      "@type": "CreativeWork",
      "name": project.title,
      "description": project.overview || project.description,
      "url": `https://maurouix.com/proyectos/${project.slug}`,
      "author": { "@id": "https://maurouix.com/#person" },
      ...(project.year ? { "dateCreated": project.year } : {}),
      ...(project.url ? { "about": { "@type": "WebSite", "url": project.url } } : {}),
      "keywords": project.tags.join(", "),
      "inLanguage": "es"
    }
  ]
})} />
```

---

## HIGH -- Corregir esta semana

### 6. Crear llms.txt
**Archivo:** `public/llms.txt`
**Tiempo:** 1 hora

```
# MauroUIX

> Mauricio Correa (MauroUIX) is a freelance web designer and UI/UX developer
> based in Colombia with over 10 years of experience. He specializes in
> WordPress development with Bricks Builder, UI/UX design in Figma, and
> corporate identity design for B2B companies in the automotive, logistics,
> and e-commerce sectors.

## Services
- Web Interface Design (UI/UX)
- Creative Website Development (WordPress, Bricks Builder, Figma)
- Corporate Identity & Branding

## Portfolio
- [AutoVenz](https://maurouix.com/proyectos/autovenz): B2B automotive parts distributor
- [Indireca](https://maurouix.com/proyectos/indireca): Colombian auto parts e-commerce
- [Inversiones GC](https://maurouix.com/proyectos/inversiones-gc): Industrial motor distributor
- [Imporlasca](https://maurouix.com/proyectos/imporlasca): International trade & logistics
- [Encanto Amatista](https://maurouix.com/proyectos/encanto-amatista): Jewelry e-commerce boutique
- [Daca Trader](https://maurouix.com/proyectos/dacatrader): B2B motor import company

## Contact
- Email: contacto@maurouix.com
- WhatsApp: +57 302 372 5631
- LinkedIn: https://linkedin.com/in/mcorread
- Website: https://maurouix.com
```

---

### 7. Habilitar compresión gzip/brotli en nginx
**Dónde:** Configuración del servidor (Coolify/nginx)
**Tiempo:** 30 min

```nginx
gzip on;
gzip_types text/html text/css application/javascript application/json image/svg+xml;
gzip_min_length 256;
```

---

### 8. Optimizar carga de fuentes
**Archivos:** `global.css`, `index.astro`, `[slug].astro`
**Tiempo:** 1 hora

Mover el `@import` de fonts en `global.css` a tags `<link>` en el `<head>` de cada página:
```html
<link rel="preconnect" href="https://fonts.googleapis.com" />
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
<link rel="stylesheet" href="https://fonts.googleapis.com/css2?family=Inter+Tight:wght@700;800;900&family=Figtree:wght@300;400;500;600&family=JetBrains+Mono:wght@400;500&display=swap" />
```

Eliminar la línea `@import url(...)` de `global.css`. Reducir variantes de peso a las que realmente se usan.

---

### 9. Unificar navegación a español
**Archivo:** `src/components/Header.astro`
**Tiempo:** 15 min

Cambiar:
- "Work" -> "Proyectos"
- "About" -> "Sobre Mí"
- "Contact" -> "Contacto"

Y en `Hero.astro`: "Scroll to explore" -> "Desliza para explorar"

---

### 10. Agregar headers de seguridad
**Dónde:** Configuración nginx (Coolify)
**Tiempo:** 30 min

```nginx
add_header Strict-Transport-Security "max-age=31536000; includeSubDomains" always;
add_header X-Content-Type-Options "nosniff" always;
add_header X-Frame-Options "SAMEORIGIN" always;
add_header Referrer-Policy "strict-origin-when-cross-origin" always;
server_tokens off;
```

---

### 11. Agregar imágenes/capturas a proyectos
**Archivos:** `[slug].astro`, `public/` o `src/assets/`
**Tiempo:** 4-6 horas

Cada proyecto debería tener 2-3 screenshots con alt text descriptivo. Esto es **crítico** para un portfolio de diseño.

---

### 12. Corregir copyright de páginas de proyecto
**Archivo:** `src/pages/proyectos/[slug].astro` (línea 217)
**Tiempo:** 5 min

Cambiar "2025" a "2026".

---

## MEDIUM -- Corregir este mes

### 13. Expandir descripciones de servicios
**Archivo:** `src/components/Services.astro`
**Tiempo:** 2 horas

Cada servicio debería tener 150-250 palabras: qué incluye, para quién es, herramientas usadas, entregables.

### 14. Expandir sección About/Bio
**Archivo:** `src/components/Philosophy.astro`
**Tiempo:** 1 hora

Incluir: educación, ciudad, industrias atendidas, cantidad de proyectos completados, metodología.

### 15. Eliminar contenido duplicado entre Servicios y Filosofía
**Archivos:** `Services.astro`, `Philosophy.astro`
**Tiempo:** 1 hora

### 16. Mejorar meta descriptions
**Archivos:** `index.astro`, `[slug].astro`
**Tiempo:** 30 min

Homepage: expandir a 120-160 caracteres con propuesta de valor clara.
Proyectos: usar `project.description` completo en vez del patrón `{title} — {meta}`.

### 17. Crear página 404 personalizada
**Archivo:** `src/pages/404.astro`
**Tiempo:** 1 hora

### 18. Agregar testimonios de clientes
**Archivos:** Nuevo componente o dentro de Contact
**Tiempo:** 3 horas (requiere recopilar feedback)

### 19. Agregar métricas/resultados a proyectos
**Archivo:** `src/data/projects.ts`
**Tiempo:** 2 horas

Agregar campos como: resultado, métricas de impacto, tiempo de entrega.

---

## LOW -- Backlog

### 20. Agregar sección FAQ en homepage
### 21. Configurar IndexNow para Bing/Yandex
### 22. Agregar cache headers para assets estáticos
### 23. Considerar self-hosting de fonts (@fontsource)
### 24. Configurar redirect de www a dominio principal
### 25. Crear presencia en YouTube (walkthroughs de proyectos)
### 26. Agregar política de privacidad

---

## Impacto Estimado

| Implementación | Score Estimado |
|----------------|---------------|
| Solo Critical (items 1-5) | 50-55/100 |
| Critical + High (items 1-12) | 65-75/100 |
| Todo Critical + High + Medium | 75-85/100 |
| Implementación completa | 85-90/100 |
