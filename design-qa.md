# Design QA — rediseño editorial MauroUIX

## Alcance y verdad visual

- Referencia editorial: `/tmp/maurouix-awwwards-reference-2026-08-11/01-awwwards-home.png` (1265 × 712 px).
- Estado anterior del sitio: `/tmp/maurouix-ux-audit-2026-08-11/01-home-desktop.png`.
- Implementación, hero escritorio: `/tmp/maurouix-design-qa/home-desktop.jpg` (1434 × 896 px; viewport 1440 × 900).
- Implementación, caso de estudio: `/tmp/maurouix-design-qa/case-desktop.jpg` (1434 × 896 px; viewport 1440 × 900).
- Implementación móvil: `/tmp/maurouix-design-qa/home-mobile.jpg` y `/tmp/maurouix-design-qa/project-mobile.jpg` (viewport 390 × 844).
- Comparación conjunta: `/tmp/maurouix-design-qa/comparison-awwwards-home.jpg`.
- La referencia se utilizó como lenguaje editorial y de composición, no como clon visual.

## Matriz de revisión

| Área | Evidencia | Resultado |
|---|---|---|
| Hero editorial | Captura 1440 × 900 | Titular, capturas reales y CTAs principales visibles; jerarquía aprobada. |
| Showcase | Navegación a `#work` y captura enfocada | Preview sticky visible; `Ver caso` y `Visitar sitio` aparecen simultáneamente. |
| Teclado | Foco sobre el segundo proyecto | El preview activo cambió de índice `0` a `1` mediante `focusin`. |
| Caso de estudio | `/proyectos/encanto-amatista` | Hero, acción al sitio, contexto, galería, CTA y navegación anterior/siguiente presentes. |
| Móvil | 390 × 844 | Tarjeta completa, imagen y ambas acciones visibles sin hover ni acordeón. |
| Menú móvil | Abrir, Escape y espera de transición | Al cerrar: `aria-expanded=false`, `aria-hidden=true`, `hidden=true`; foco devuelto al botón. |
| Reflow | Medición en 390 px | Sin overflow horizontal (`scrollWidth` no supera el viewport útil). |
| Consola | Logs de la sesión local | Sin errores de JavaScript; solo aviso no bloqueante de Microsoft Clarity. |
| Movimiento reducido | CSS y flujo GSAP | Parallax y revelados complejos anulados bajo `prefers-reduced-motion`. |
| Build | `pnpm build` | 9 rutas estáticas y 240 variantes optimizadas generadas correctamente. |

## Hallazgos y correcciones

1. **P1 — titular móvil recortado:** a 390 px, “Transformando” excedía ligeramente el ancho disponible. Se redujo la escala fluida móvil de `15vw` a `13.2vw` y se repitió la captura. Resultado: texto completo y sin overflow.
2. **P2 — validación temporal del menú:** el atributo `hidden` se aplica al finalizar la transición de cierre. Se verificó después de 520 ms y el menú quedó fuera del árbol de foco correctamente.

No quedaron hallazgos P0, P1 o P2 abiertos.

final result: passed
