---
name: build-precision-dashboard-ui
description: Build, restyle, or visually repair desktop SaaS, admin, analytics, monitoring, operations, task, table, map, editor, and developer-tool interfaces in the bundled precision enterprise dashboard style. Use when the user requests a dense dark dashboard, control-room UI, data-heavy workspace, or strict fidelity to these visual references, including when code works but the rendered UI has drifted. Preserve the existing product stack and behavior. Treat dark mode as the default; use the bundled light monitoring variant only when the user requests light mode or the existing product requires it.
---

# Build Precision Dashboard UI

Build the product in the visual language of the bundled references. Translate the user's content into that system; do not reinterpret the system itself.

## Enforce the reference hierarchy

1. Inspect every relevant image in `assets/inspiration/` with an image-viewing tool before designing or editing.
2. Read `references/style-system.md` and `references/layout-archetypes.md`.
3. Choose one image as the primary reference according to the product's information architecture. Use at most two supporting images only for details the primary image does not answer.
4. Let the primary reference override personal taste, framework defaults, design trends, and generic component-library styling.
5. Treat the colorful or textured area outside an app window as presentation framing, not product UI. Reproduce it only when the user asks for a showcase mockup.

## Establish a visual contract

Before implementation, state or record a compact contract covering:

- primary reference and layout archetype
- viewport and responsive scope
- panel topology and approximate proportions
- dark or light theme
- type mode: neutral sans or technical monospace
- one product accent plus semantic status colors
- density target, border treatment, and radius scale
- required charts, tables, maps, lists, or detail panes

Do not start from a blank aesthetic. Map the product requirements into this contract.

## Build in fidelity order

1. Preserve the existing framework, routes, data flow, and working interactions.
2. Rebuild the macro shell first: rails, sidebar, top bar, content regions, split panes, and overflow behavior.
3. Apply shared tokens and primitives. Adapt `assets/precision-ui-tokens.css` rather than scattering one-off values.
4. Implement repeated components: navigation rows, controls, KPI cards, table rows, chips, list items, and panels.
5. Add data visualizations and domain content. Keep labels realistic and information-dense.
6. Add hover, active, focus, selected, loading, empty, and error states without changing the visual grammar.
7. Add responsive behavior only after the desktop target matches. Do not turn a desktop control surface into a mobile card feed unless requested.

## Preserve the defining style

- Use near-black layered surfaces, hairline separators, compact controls, quiet typography, restrained color, and desktop-first density.
- Prefer contiguous panels and grid lines over floating card collections.
- Keep controls small, labels short, and hierarchy driven by alignment and contrast.
- Use one consistent outline icon family. Use avatars or logos only where identity matters.
- Use status color only for status, alerts, active routes, selected data, or a primary action.
- Keep the visual tone serious, operational, and precise.

Never introduce glassmorphism, neon glow, glossy gradients, oversized hero typography, soft consumer-app cards, excessive pills, large empty zones, emoji icons, playful illustration, or unrelated visual motifs.

## Pass both completion gates

### Code gate

- Run the project's relevant typecheck, lint, tests, and build.
- Exercise the changed interactions and preserve existing behavior.
- Check overflow, keyboard focus, semantic labels, and contrast.

### Visual gate

1. Render the actual UI at the target desktop viewport, normally 1440×900 or the reference aspect ratio.
2. Capture a screenshot and compare it with the primary reference using `references/visual-qa.md`.
3. Fix the largest structural mismatch first, then density, surface contrast, typography, controls, and micro-details.
4. Repeat until both gates pass. Do not claim completion from code inspection alone.

If rendering is impossible, report the visual gate as unverified and explain the exact blocker. After three targeted repair attempts with no material improvement, stop patching symptoms, identify the faulty shared primitive or structural constraint, and report the smallest next action that can unblock fidelity.

## Use the bundled resources

- `references/layout-archetypes.md`: choose the closest reference and layout.
- `references/style-system.md`: apply exact tokens, spacing, typography, controls, and visualization rules.
- `references/visual-qa.md`: perform screenshot-based review and repair.
- `assets/inspiration/`: inspect the nine source images; never replace them with generic inspiration.
- `assets/precision-ui-tokens.css`: use as a portable starting token layer.
