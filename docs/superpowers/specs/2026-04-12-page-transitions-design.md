# Page Transitions — Design Spec

**Date:** 2026-04-12
**Project:** MauroUIX Portfolio (Astro 5 static site)

---

## Summary

Add directional horizontal slide transitions between the home page (`/`) and project detail pages (`/proyectos/[slug]`). Duration: 650ms. Built on Astro's native View Transitions API.

---

## Decisions

| Dimension | Decision | Rationale |
|---|---|---|
| Style | Horizontal slide | Selected by user |
| Direction | Directional (contextual) | Spatial continuity — user feels navigation depth |
| Duration | 650ms | Fluent without feeling slow; standard for premium portfolios |
| Easing | `cubic-bezier(0.77, 0, 0.175, 1)` | Strong ease-in/out, already used in the project's GSAP animations |
| Mechanism | Astro View Transitions | Native Astro 5 integration, automatic fallback for older browsers |

---

## Direction Logic

Direction is determined by comparing origin and destination URLs in the `astro:before-preparation` event:

- **Forward** (`/` → `/proyectos/*`): new page enters from the right, old page exits to the left
- **Backward** (`/proyectos/*` → `/`): new page enters from the left, old page exits to the right
- **Any other pair** (e.g., project → project): defaults to forward

The detected direction is written to `document.documentElement.dataset.direction` (`"forward"` | `"backward"`). CSS `::view-transition-new(root)` and `::view-transition-old(root)` pseudo-elements read this attribute to apply the correct keyframe animation.

---

## CSS Animations

Four keyframes in `global.css`:

```css
/* Forward: home → project */
@keyframes slide-from-right { from { transform: translateX(100%); } to { transform: translateX(0); } }
@keyframes slide-to-left    { from { transform: translateX(0); }   to { transform: translateX(-100%); } }

/* Backward: project → home */
@keyframes slide-from-left  { from { transform: translateX(-100%); } to { transform: translateX(0); } }
@keyframes slide-to-right   { from { transform: translateX(0); }    to { transform: translateX(100%); } }
```

Applied via:

```css
[data-direction="forward"]  ::view-transition-new(root) { animation: slide-from-right 650ms ... }
[data-direction="forward"]  ::view-transition-old(root) { animation: slide-to-left    650ms ... }
[data-direction="backward"] ::view-transition-new(root) { animation: slide-from-left  650ms ... }
[data-direction="backward"] ::view-transition-old(root) { animation: slide-to-right   650ms ... }
```

`prefers-reduced-motion` must override all `::view-transition-*` animations to an instant swap.

---

## GSAP Integration

Astro's View Transitions intercepts navigation — after the first load, `DOMContentLoaded` no longer fires on subsequent page visits. All GSAP initialization must migrate to the `astro:page-load` event, which fires after every transition (including the first load).

ScrollTrigger instances must be killed before each swap to prevent memory leaks and stale triggers from the previous page. This is done in the `astro:before-swap` event.

### Event lifecycle used

| Event | Action |
|---|---|
| `astro:before-preparation` | Detect direction, set `data-direction` on `<html>` |
| `astro:before-swap` | Kill all ScrollTrigger instances |
| `astro:page-load` | Re-run all GSAP initialization (replaces `DOMContentLoaded`) |

---

## Files Changed

| File | Change |
|---|---|
| `src/pages/index.astro` | Import and add `<ViewTransitions />` in `<head>`; add direction detection + ScrollTrigger cleanup script; migrate `DOMContentLoaded` → `astro:page-load` |
| `src/pages/proyectos/[slug].astro` | Import and add `<ViewTransitions />`; add direction detection + cleanup script; migrate `DOMContentLoaded` → `astro:page-load` |
| `src/styles/global.css` | Add slide keyframes; add `::view-transition-*` rules; add `prefers-reduced-motion` override |

---

## Browser Support

| Browser | Support |
|---|---|
| Chrome 111+ / Edge 111+ | Full View Transitions API |
| Safari 18+ | Full View Transitions API |
| Safari < 18 / Firefox | Astro's JS fallback — instant swap, no animation, no breakage |

---

## Out of Scope

- Transitions between two project pages (`/proyectos/a` → `/proyectos/b`) — defaults to forward slide; no special handling needed
- Shared element transitions (e.g., project title morphing from list to detail page)
- Loading indicators during transition
