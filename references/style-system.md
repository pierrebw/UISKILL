# Precision enterprise dashboard style system

## Visual character

Create a dense, desktop-first enterprise control surface: calm, severe, high-information, and meticulously aligned. Let hierarchy come from panel geometry, 1 px separators, typographic weight, and small contrast steps. Decoration is subordinate to function.

## Core tokens

Use these values as the default baseline. Adjust only when an existing brand token or the chosen primary reference clearly requires it.

| Role | Dark default | Light alternate |
|---|---:|---:|
| Canvas | `#09090B` | `#F7F7F8` |
| Sidebar | `#0E0E10` | `#FFFFFF` |
| Panel | `#121214` | `#FFFFFF` |
| Elevated panel | `#17171A` | `#FAFAFB` |
| Hover | `#1C1C20` | `#F2F2F4` |
| Selected | `#242429` | `#ECECEF` |
| Subtle border | `#1D1D21` | `#ECECEF` |
| Strong border | `#2A2A2F` | `#DEDEE3` |
| Primary text | `#F2F2F3` | `#17171A` |
| Secondary text | `#A5A5AB` | `#5E5E66` |
| Muted text | `#6E6E76` | `#9898A1` |

Select one product accent: blue `#3B82F6`, orange `#FF7A1A`, or green `#36C66A`. Follow an existing brand accent when supplied. Do not distribute multiple product accents across unrelated controls.

Reserve semantic colors for state: success `#36C66A`, warning `#F3B61F`, danger `#FF414D`, information `#3B82F6`. Keep highly saturated color below roughly 5% of the visible interface.

## Typography

- Use Inter, Geist, SF Pro, or the project's closest neutral grotesk for task, analytics, operations, and editor layouts.
- Use Geist Mono, IBM Plex Mono, or an existing product monospace for the engineering-console archetype.
- Use 13–14 px for navigation and body, 11–12 px for metadata and uppercase labels, 15–18 px for panel titles, and 28–40 px only for a true page title.
- Prefer weights 400, 500, and 600. Reserve 700 for a major value or title.
- Use line-height 1.25–1.5. Use tabular numerals for metrics, timestamps, ranks, and table values.
- Keep labels sentence case except compact data headers, which may be uppercase with `0.05em–0.08em` tracking.

## Geometry and density

- Build a fixed desktop shell with independently scrolling regions when the product benefits from persistent context.
- Use a 60–76 px icon rail, 240–320 px sidebar, 56–72 px top bar, and 360–520 px detail pane.
- Use 20–28 px page padding, 12–20 px panel padding, and 8–16 px compact gaps.
- Use 48–60 px table/list rows, 32–38 px buttons, 36–42 px inputs, 28–34 px chips, and 24–32 px avatars.
- Use 8–12 px component radii and 6–10 px nested radii. Full pills are limited to status chips, segmented toggles, and compact filters.
- Use a 24–40 px outer-shell radius only for a framed showcase image. A normal in-product viewport should be flush.
- Prefer bordering adjacent regions over adding broad whitespace. Nested cards must earn their boundary.

## Surfaces and depth

- Separate hierarchy with one or two luminance steps and 1 px borders.
- Keep shadows absent or extremely soft. Never use bright glows.
- Use selected rows as a quiet single-tone fill. Add a thin active edge only when the reference uses one.
- Keep canvas, sidebar, content, and elevated controls visibly distinct without turning each into a floating card.

## Components

- Use 16–18 px outline icons at 1.5–2 px stroke. Keep icon boxes aligned even when labels vary.
- Use compact buttons with direct labels. Make one primary action visually dominant; keep secondary actions outlined or tonal.
- Use search and filters as low-profile controls with border and subtle fill, not large form fields.
- Use thin dividers and precise column alignment in tables. Right-align comparable numbers where useful.
- Use small status chips with a tinted surface and colored icon/text. Avoid giving every tag a vivid fill.
- Use 24–32 px avatars; group name and activity text tightly.
- Keep motion between 120–180 ms with standard ease-out. Avoid bounce, parallax, and decorative entrance sequences.

## Data visualization

- Use hairline gridlines and subdued axes. Keep plot backgrounds continuous with the panel.
- Use 1.5–2 px line strokes and flat fills. Avoid glossy gradients, fake 3D, and ornamental shadows.
- Use color to encode series or state consistently. Label key values directly when it reduces legend hunting.
- Use tabular numerals, compact legends, and restrained tooltips matching the panel surface.
- For maps, use dark muted tiles, dim place labels, and bright routes or incident markers. Keep overlays compact and legible.
- For heatmaps and dense tables, use tonal intensity within a single hue family plus sparse semantic exceptions.

## Light alternate

Use the light system only when requested or required by an existing product. Preserve the same compact geometry, thin borders, restrained orange/accent use, edge-to-edge chart panels, and low-shadow treatment. Do not convert it into a spacious consumer dashboard.

## Prohibited drift

Reject these deviations:

- glass blur, translucent cards, neon glow, or aurora effects inside the product
- gradients used as generic decoration
- default component-library spacing or roundness left uncorrected
- huge headings, centered hero compositions, or marketing-page structure
- broad card grids when a split pane, table, list, or contiguous dashboard is more faithful
- inconsistent icon families, emoji icons, excessive badges, or decorative avatars
- low information density, oversized controls, or repeated labels that weaken scanability
- copying the colorful fabric, sky, or gradient outside a showcased app shell into the app itself
