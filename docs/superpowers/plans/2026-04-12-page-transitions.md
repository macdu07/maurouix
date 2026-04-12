# Page Transitions Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Add directional horizontal slide transitions (650ms) between home `/` and project pages `/proyectos/[slug]` using Astro's View Transitions API.

**Architecture:** `<ViewTransitions />` intercepts all `<a>` clicks. Direction (forward/backward) is detected in `astro:before-preparation` and written to `html[data-direction]`. CSS `::view-transition-*` pseudo-elements read that attribute to play the correct slide keyframe. GSAP re-initializes on every `astro:page-load`; ScrollTrigger instances are killed in `astro:before-swap` to prevent leaks. A `window.__transitionsInit` flag prevents duplicate event-listener registration when the same page is visited more than once.

**Tech Stack:** Astro 5 `astro:transitions`, CSS View Transitions API, GSAP + ScrollTrigger

---

## File Map

| File | What changes |
|---|---|
| `src/styles/global.css` | Add 4 slide keyframes + `::view-transition-*` rules + reduced-motion override |
| `src/pages/index.astro` | Add `ViewTransitions` import + component; add lifecycle script block; change `DOMContentLoaded` → `astro:page-load` |
| `src/pages/proyectos/[slug].astro` | Same ViewTransitions wiring; wrap module-level GSAP code in `astro:page-load` |

---

## Task 1 — CSS: slide keyframes and view-transition rules

**Files:**
- Modify: `src/styles/global.css`

- [ ] **Step 1: Append slide keyframes and transition rules**

Add the following block at the very end of `src/styles/global.css`:

```css
/* ── View Transitions: directional slide ── */
@keyframes slide-from-right { from { transform: translateX(100%); } to { transform: translateX(0); } }
@keyframes slide-to-left    { from { transform: translateX(0); }   to { transform: translateX(-100%); } }
@keyframes slide-from-left  { from { transform: translateX(-100%); } to { transform: translateX(0); } }
@keyframes slide-to-right   { from { transform: translateX(0); }   to { transform: translateX(100%); } }

[data-direction="forward"]  ::view-transition-new(root) { animation: slide-from-right 650ms cubic-bezier(0.77,0,0.175,1) both; }
[data-direction="forward"]  ::view-transition-old(root) { animation: slide-to-left    650ms cubic-bezier(0.77,0,0.175,1) both; }
[data-direction="backward"] ::view-transition-new(root) { animation: slide-from-left  650ms cubic-bezier(0.77,0,0.175,1) both; }
[data-direction="backward"] ::view-transition-old(root) { animation: slide-to-right   650ms cubic-bezier(0.77,0,0.175,1) both; }

@media (prefers-reduced-motion: reduce) {
  ::view-transition-old(root),
  ::view-transition-new(root) {
    animation: none !important;
    mix-blend-mode: normal;
  }
}
```

- [ ] **Step 2: Verify build**

```bash
npm run build
```
Expected: `[build] Complete!` with no errors.

- [ ] **Step 3: Commit**

```bash
git add src/styles/global.css
git commit -m "feat: add view-transition slide keyframes and direction rules"
```

---

## Task 2 — index.astro: ViewTransitions + lifecycle handlers

**Files:**
- Modify: `src/pages/index.astro`

- [ ] **Step 1: Add ViewTransitions import to frontmatter**

Replace the existing frontmatter block (between the `---` fences) with:

```astro
---
import '../styles/global.css';
import Header from '../components/Header.astro';
import Hero from '../components/Hero.astro';
import Projects from '../components/Projects.astro';
import Services from '../components/Services.astro';
import Philosophy from '../components/Philosophy.astro';
import Contact from '../components/Contact.astro';
import { ViewTransitions } from 'astro:transitions';
---
```

- [ ] **Step 2: Add `<ViewTransitions />` to the head**

Inside `<head>`, directly before the closing `</head>` tag, add:

```html
    <ViewTransitions />
  </head>
```

- [ ] **Step 3: Add lifecycle handlers + replace `DOMContentLoaded` in the existing GSAP script**

In the existing `<!-- GSAP Scripts -->` block, directly after `gsap.registerPlugin(ScrollTrigger);` and before the first function definition, insert the lifecycle handler block:

```js
      // ─── View Transition lifecycle (set up once per browser session) ───
      if (!(window as any).__transitionsInit) {
        (window as any).__transitionsInit = true;

        document.addEventListener('astro:before-preparation', (e: any) => {
          const from: string = e.from.pathname;
          const to: string = e.to.pathname;
          const backward = from.startsWith('/proyectos/') && !to.startsWith('/proyectos/');
          document.documentElement.dataset.direction = backward ? 'backward' : 'forward';
        });

        document.addEventListener('astro:before-swap', () => {
          ScrollTrigger.getAll().forEach((t) => t.kill());
        });
      }
```

Then, in the same script block, find:

```js
      document.addEventListener('DOMContentLoaded', () => {
```

Replace with:

```js
      document.addEventListener('astro:page-load', () => {
```

No other changes inside the listener are needed.

- [ ] **Step 4: Build and verify**

```bash
npm run build
```
Expected: `[build] Complete!` with no errors.

- [ ] **Step 5: Commit**

```bash
git add src/pages/index.astro
git commit -m "feat: wire ViewTransitions and lifecycle handlers in index.astro"
```

---

## Task 3 — [slug].astro: ViewTransitions + wrap GSAP in astro:page-load

**Files:**
- Modify: `src/pages/proyectos/[slug].astro`

- [ ] **Step 1: Add ViewTransitions import to frontmatter**

Replace the existing frontmatter block with:

```astro
---
import '../../styles/global.css';
import Header from '../../components/Header.astro';
import { projects } from '../../data/projects';
import type { Project } from '../../data/projects';
import { ViewTransitions } from 'astro:transitions';

export function getStaticPaths() {
  return projects.map((project) => ({
    params: { slug: project.slug },
    props: { project },
  }));
}

interface Props {
  project: Project;
}

const { project } = Astro.props;
const currentIndex = projects.findIndex((p) => p.slug === project.slug);
const prevProject = currentIndex > 0 ? projects[currentIndex - 1] : null;
const nextProject = currentIndex < projects.length - 1 ? projects[currentIndex + 1] : null;
---
```

- [ ] **Step 2: Add `<ViewTransitions />` to the head**

Inside `<head>`, directly before the closing `</head>` tag, add:

```html
    <ViewTransitions />
  </head>
```

- [ ] **Step 3: Replace the entire `<script>` block**

Find the existing `<script>` block (starting at `<script>` and ending at `</script>` near the bottom of the file) and replace it entirely with:

```html
    <script>
      import { gsap } from 'gsap';
      import { ScrollTrigger } from 'gsap/ScrollTrigger';

      gsap.registerPlugin(ScrollTrigger);

      // ─── Transition lifecycle (once per browser session) ───
      if (!(window as any).__transitionsInit) {
        (window as any).__transitionsInit = true;

        document.addEventListener('astro:before-preparation', (e: any) => {
          const from: string = e.from.pathname;
          const to: string = e.to.pathname;
          const backward = from.startsWith('/proyectos/') && !to.startsWith('/proyectos/');
          document.documentElement.dataset.direction = backward ? 'backward' : 'forward';
        });

        document.addEventListener('astro:before-swap', () => {
          ScrollTrigger.getAll().forEach((t) => t.kill());
        });
      }

      // ─── Page initialization (runs after every navigation) ───
      document.addEventListener('astro:page-load', () => {
        const reducedMotion = window.matchMedia('(prefers-reduced-motion: reduce)').matches;

        if (!reducedMotion) {
          // Grid parallax
          gsap.to('#grid-bg', {
            backgroundPosition: '30px 200px',
            ease: 'none',
            scrollTrigger: {
              trigger: 'body',
              start: 'top top',
              end: 'bottom bottom',
              scrub: 1.5,
            },
          });

          // Scroll reveals
          gsap.utils.toArray<HTMLElement>('.reveal-up').forEach((el) => {
            gsap.fromTo(
              el,
              { opacity: 0, y: 30 },
              {
                opacity: 1,
                y: 0,
                duration: 0.8,
                ease: 'power2.out',
                scrollTrigger: {
                  trigger: el,
                  start: 'top 88%',
                  toggleActions: 'play none none none',
                },
              }
            );
          });
        } else {
          document.querySelectorAll('.reveal-up').forEach((el) => {
            gsap.set(el, { opacity: 1, y: 0 });
          });
        }

        // Header blur on scroll
        const header = document.querySelector('header');
        if (header) {
          ScrollTrigger.create({
            start: '100px top',
            onEnter: () => {
              (header as HTMLElement).style.background = 'rgba(13,13,13,0.85)';
              (header as HTMLElement).style.backdropFilter = 'blur(12px)';
            },
            onLeaveBack: () => {
              (header as HTMLElement).style.background = '';
              (header as HTMLElement).style.backdropFilter = '';
            },
          });
        }
      });
    </script>
```

Note: the original `[slug].astro` script ran GSAP code at module level (no wrapper). This step wraps everything in `astro:page-load` so it re-runs on every navigation to a project page. The `gsap.registerPlugin` call stays at module level — GSAP handles duplicate calls gracefully.

- [ ] **Step 4: Build and verify**

```bash
npm run build
```
Expected: `[build] Complete!` with no errors.

- [ ] **Step 5: Commit**

```bash
git add src/pages/proyectos/[slug].astro
git commit -m "feat: wire ViewTransitions and astro:page-load in slug.astro"
```

---

## Task 4 — Manual smoke test

- [ ] **Step 1: Start dev server**

```bash
npm run dev
```

- [ ] **Step 2: Test forward transition (home → project)**

1. Open `http://localhost:4321`
2. Click any project in the list
3. Expected: page slides LEFT — current page exits left, project page enters from right
4. Expected: GSAP reveal animations play on the project page

- [ ] **Step 3: Test backward transition (project → home)**

1. From a project page, click "MauroUIX" logo or any nav link (Proyectos, Servicios, etc.)
2. Expected: page slides RIGHT — project page exits right, home enters from left
3. Expected: GSAP animations play, scroll goes to the correct section (hash)

- [ ] **Step 4: Test project → project transition**

1. From a project page, click "Siguiente →" to go to the next project
2. Expected: forward slide (left) since both URLs start with `/proyectos/`

- [ ] **Step 5: Test reduced-motion**

1. In macOS: System Settings → Accessibility → Display → Reduce Motion: ON
2. Reload and navigate between pages
3. Expected: instant swap with no slide animation, content still appears correctly

- [ ] **Step 6: Final commit if any fixes were needed**

```bash
git add -p
git commit -m "fix: post-smoke-test adjustments to page transitions"
```
