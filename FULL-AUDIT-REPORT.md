# Auditoría SEO Completa -- maurouix.com

**Fecha:** 2026-03-26
**URL:** https://maurouix.com
**Tipo de negocio:** Portfolio freelance -- Diseñador Web & Desarrollador UI/UX
**Stack:** Astro 5 (estático), Tailwind CSS v4, GSAP + ScrollTrigger
**Servidor:** nginx/1.29.7
**Idioma:** Español (es)
**Páginas:** 7 (homepage + 6 páginas de detalle de proyecto)

---

## Resumen Ejecutivo

### SEO Health Score: 32 / 100

| Categoría | Peso | Puntaje | Ponderado |
|-----------|------|---------|-----------|
| SEO Técnico | 22% | 28/100 | 6.2 |
| Calidad de Contenido | 23% | 52/100 | 12.0 |
| SEO On-Page | 20% | 40/100 | 8.0 |
| Schema / Datos Estructurados | 10% | 0/100 | 0.0 |
| Rendimiento (CWV) | 10% | 45/100 | 4.5 |
| AI Search Readiness | 10% | 18/100 | 1.8 |
| Imágenes | 5% | 5/100 | 0.25 |
| **TOTAL** | | | **32.7** |

### Top 5 Problemas Críticos

1. **No existe robots.txt** -- Los motores de búsqueda no tienen directivas de rastreo
2. **No existe sitemap.xml** -- Los buscadores no pueden descubrir todas las páginas eficientemente
3. **Sin etiquetas canonical** -- Riesgo de contenido duplicado en todas las páginas
4. **Cero datos estructurados (JSON-LD)** -- Invisible para rich results y paneles de conocimiento
5. **Sin Open Graph / Twitter Cards** -- Las previsualizaciones al compartir en redes sociales están en blanco

### Top 5 Quick Wins

1. Crear `public/robots.txt` (30 min)
2. Instalar `@astrojs/sitemap` y generar sitemap (30 min)
3. Agregar etiquetas `<link rel="canonical">` a todas las páginas (15 min)
4. Agregar meta tags Open Graph y Twitter Card (1 hora)
5. Crear `public/llms.txt` para visibilidad en AI (1 hora)

---

## 1. SEO Técnico (28/100)

### 1.1 Rastreabilidad -- FALLA

| Check | Estado | Detalle |
|-------|--------|---------|
| robots.txt | FALLA | Retorna 404. No existe. |
| sitemap.xml | FALLA | Retorna 404. No hay integración de sitemap en Astro. |
| Meta robots | WARN | No hay `<meta name="robots">`. Funciona por defecto como index/follow pero es implícito. |
| Gestión de crawlers AI | FALLA | Sin robots.txt = sin reglas para GPTBot, ClaudeBot, etc. |

### 1.2 Indexabilidad -- FALLA

| Check | Estado | Detalle |
|-------|--------|---------|
| Etiquetas canonical | FALLA | No hay `<link rel="canonical">` en ninguna página. |
| Title tags | OK | Homepage y páginas de proyecto tienen títulos únicos y descriptivos. |
| Meta descriptions | OK | Presentes en todas las páginas con contenido relevante. |
| Atributo lang | OK | `<html lang="es">` correctamente configurado. |

### 1.3 Seguridad -- FALLA

| Check | Estado |
|-------|--------|
| HTTPS | OK |
| Redirect HTTP a HTTPS | WARN -- Usa 307 (temporal) en vez de 301 (permanente) |
| HSTS | FALLA -- No presente |
| X-Content-Type-Options | FALLA -- No presente |
| X-Frame-Options | FALLA -- No presente |
| Content-Security-Policy | FALLA -- No presente |
| Referrer-Policy | FALLA -- No presente |
| Versión del servidor expuesta | WARN -- `nginx/1.29.7` visible |

### 1.4 Estructura de URLs -- OK

| Check | Estado | Detalle |
|-------|--------|---------|
| URLs limpias | OK | `/proyectos/autovenz` -- limpias, semánticas, lowercase |
| Jerarquía | OK | Estructura lógica: `/proyectos/{slug}` |
| Links internos | OK | Páginas de proyecto enlazan a `/#work` y a proyecto anterior/siguiente |
| **Nav en páginas de proyecto** | **WARN** | **Los links de navegación apuntan a `#work`, `#services`, etc. que son anchors del homepage. Desde una página de proyecto no funcionan correctamente.** |

### 1.5 Mobile -- OK

| Check | Estado |
|-------|--------|
| Viewport meta | OK |
| CSS responsive | OK -- Usa prefijos de Tailwind (`md:`, `sm:`) |
| Touch targets | OK |
| Tipografía fluida | OK -- Usa `clamp()` |
| Menú mobile | OK |

### 1.6 Compresión -- FALLA

El servidor **no retorna** `content-encoding: gzip` ni `br`. HTML (26 KB), CSS (20 KB) y ScrollTrigger JS (112 KB) se sirven sin comprimir.

---

## 2. Calidad de Contenido (52/100)

### 2.1 E-E-A-T

| Factor | Puntaje | Evaluación |
|--------|---------|------------|
| Experience (Experiencia) | 55/100 | Dice "+10 años" pero no hay evidencia verificable, sin proceso documentado, sin antes/después |
| Expertise (Expertise) | 50/100 | Skills listados pero sin certificaciones, sin blog, sin contenido educativo |
| Authoritativeness (Autoridad) | 35/100 | Cero señales de terceros: sin testimonios, sin logos de clientes, sin menciones de prensa |
| Trustworthiness (Confianza) | 60/100 | Contacto real presente. Pero: sin política de privacidad, sin dirección física, inconsistencia en copyright |

**E-E-A-T Compuesto: 50/100**

### 2.2 Conteo de Palabras

| Página | Palabras Est. | Mínimo Recomendado | Estado |
|--------|---------------|---------------------|--------|
| Homepage (todas las secciones) | ~280 | 500 | POR DEBAJO |
| Proyecto: AUTOVENZ | ~180 | 300 | POR DEBAJO |
| Proyecto: INDIRECA | ~170 | 300 | POR DEBAJO |
| Proyecto: INVERSIONES GC | ~175 | 300 | POR DEBAJO |
| Proyecto: IMPORLASCA | ~170 | 300 | POR DEBAJO |
| Proyecto: ENCANTO AMATISTA | ~185 | 300 | POR DEBAJO |
| Proyecto: DACA TRADER | ~180 | 300 | POR DEBAJO |

**0 de 7 páginas cumplen el mínimo de contenido.**

### 2.3 Contenido Delgado Detectado

- **Descripciones de servicios:** ~20 palabras cada una. Demasiado cortas para SEO.
- **Pilares de filosofía:** ~10 palabras cada uno. Texto duplicado con las descripciones de servicios.
- **Sección About/Bio:** Solo 2 párrafos (~60 palabras). Sin educación, industrias, metodología o diferenciadores.
- **Páginas de proyecto:** Overview + challenge son párrafos individuales. Sin resultados, métricas, ni capturas de pantalla.

### 2.4 Contenido Duplicado

Texto literalmente repetido entre Servicios y Filosofía:
- Servicio: *"Diseño web intuitivo y fácil de usar para una navegación fluida"*
- Pilar: *"Diseño web intuitivo y fácil de usar para una navegación fluida"*

### 2.5 Inconsistencia de Idioma

La navegación usa inglés ("Work", "About", "Contact") mientras todo el contenido es en español. El "Scroll to explore" del Hero también está en inglés. Esto contradice `lang="es"`.

---

## 3. SEO On-Page (40/100)

### 3.1 Title Tags

| Página | Title | Largo | Estado |
|--------|-------|-------|--------|
| Homepage | MauroUIX -- Diseñador Web & Desarrollador UI/UX | 49 chars | OK |
| Proyectos | {title} . MauroUIX | Variable | OK |

### 3.2 Meta Descriptions

| Página | Descripción | Largo | Estado |
|--------|-------------|-------|--------|
| Homepage | MauroUIX -- Diseñador Web & Desarrollador UI/UX. Transformando ideas en experiencias digitales. | 81 chars | CORTA (recomendado 120-160) |
| Proyectos | {title} -- {meta} . MauroUIX | ~40-50 chars | MUY CORTA |

### 3.3 Estructura de Headings

**Homepage:**
- H1: "Transformando Ideas en Experiencias Digitales" -- OK (pero "Diseño Web & Desarrollo UI/UX" aparece como `<p>`, no como heading)
- H2: "PROYECTOS DESTACADOS", "SERVICIOS", "MI FILOSOFÍA", "COLABOREMOS" -- OK
- H3: Nombres de proyecto y servicios -- OK

**Páginas de proyecto:**
- H1: Nombre del proyecto -- OK
- Sin H2 descriptivos -- los section labels usan `<p class="section-label">` en vez de headings

### 3.4 Open Graph / Social

| Tag | Estado |
|-----|--------|
| og:title | FALTA |
| og:description | FALTA |
| og:image | FALTA |
| og:url | FALTA |
| og:type | FALTA |
| twitter:card | FALTA |
| twitter:title | FALTA |
| twitter:image | FALTA |

---

## 4. Schema / Datos Estructurados (0/100)

**Estado actual:** CERO datos estructurados en todo el sitio.

**Schemas recomendados:**

| Schema | Página | Beneficio |
|--------|--------|-----------|
| Person + ProfilePage | Homepage | Panel de conocimiento, citaciones AI |
| ProfessionalService | Homepage | Visibilidad en búsqueda local/servicios |
| WebSite | Homepage | Elegibilidad para sitelinks |
| BreadcrumbList | Páginas de proyecto | Rich results de migas de pan |
| CreativeWork | Cada proyecto | Portfolio como obras creativas estructuradas |

---

## 5. Rendimiento (45/100)

| Métrica | Riesgo | Detalle |
|---------|--------|---------|
| LCP | MEDIO | Fonts cargadas via CSS `@import` crean cadena de bloqueo: HTML -> CSS -> Google Fonts CSS -> Font files |
| INP | BAJO | Interactividad JS mínima. GSAP es scroll-triggered. |
| CLS | MEDIO | Sin `width`/`height` en elementos de layout. Font swap causa CLS. |
| Compresión | ALTO | Sin gzip/brotli. 112 KB de ScrollTrigger sin comprimir. |
| Carga de fonts | ALTO | 3 familias (Inter Tight, Figtree, JetBrains Mono) con 15+ variantes via `@import`. Peor método de carga. |
| Cache | MEDIO | Sin headers `Cache-Control` en assets estáticos. |

---

## 6. Imágenes (5/100)

| Check | Estado |
|-------|--------|
| Imágenes en el sitio | FALLA -- El sitio no tiene NINGUNA etiqueta `<img>`. Cero imágenes. |
| Alt text | N/A |
| Formatos optimizados | N/A |
| Lazy loading | N/A |

**Para un portfolio de diseño web, no tener imágenes es un problema crítico de credibilidad y SEO.**

---

## 7. AI Search Readiness (18/100)

| Dimensión | Puntaje |
|-----------|---------|
| Citabilidad | 12/100 |
| Legibilidad estructural | 30/100 |
| Contenido multi-modal | 5/100 |
| Autoridad y señales de marca | 15/100 |
| Accesibilidad técnica | 26/100 |

### Problemas principales:

- **llms.txt:** No existe (404)
- **robots.txt:** No existe (404) -- sin directivas para crawlers AI
- **Citabilidad:** Ningún pasaje del sitio responde directamente a una consulta que un usuario haría
- **Sin imágenes, video o contenido multimedia**
- **Sin FAQ o contenido en formato pregunta-respuesta**

### Visibilidad por plataforma AI:

| Plataforma | Puntaje Estimado |
|------------|-----------------|
| Google AI Overviews | 10/100 |
| ChatGPT Search | 8/100 |
| Perplexity | 12/100 |
| Bing Copilot | 10/100 |

---

## 8. Hallazgos Adicionales

| Hallazgo | Severidad |
|----------|-----------|
| Footer de homepage dice "2026" pero páginas de proyecto dicen "2025" | Baja |
| Sin página 404 personalizada | Media |
| Contenido del accordion oculto por defecto (`height: 0`) | Media |
| Sin `www` redirect configurado | Baja |
| Sin IndexNow para Bing/Yandex | Baja |

---

## Archivos Relevantes del Proyecto

| Archivo | Qué Modificar |
|---------|---------------|
| `src/pages/index.astro` | Agregar canonical, OG tags, JSON-LD, meta description más larga |
| `src/pages/proyectos/[slug].astro` | Agregar canonical, OG tags, JSON-LD, fix copyright year |
| `src/styles/global.css` | Mover `@import` de fonts a `<link>` tags en HTML |
| `src/components/Header.astro` | Cambiar nav links a español, fix anchors para project pages |
| `src/components/Hero.astro` | Cambiar "Scroll to explore" a español |
| `src/components/Services.astro` | Expandir descripciones de servicios |
| `src/components/Philosophy.astro` | Eliminar texto duplicado, expandir contenido |
| `astro.config.mjs` | Agregar `site` y `@astrojs/sitemap` |
| `public/` | Crear robots.txt, llms.txt, OG image |
