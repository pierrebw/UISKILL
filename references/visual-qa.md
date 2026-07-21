# Visual QA and repair

Compare the running product with the chosen primary reference at the same or nearest practical aspect ratio. Judge the rendered result, not just the component tree or CSS.

## Review order

1. **Topology:** Verify the number, position, and scrolling behavior of rails, sidebars, top bars, panes, tables, charts, and inspectors.
2. **Proportion:** Compare sidebar width, header height, content splits, card spans, and dominant work area.
3. **Density:** Compare visible row count, control height, gaps, padding, and text scale.
4. **Surfaces:** Compare black/gray layering, border visibility, selection fills, and absence of unnecessary shadow.
5. **Typography:** Compare hierarchy, weights, line-height, truncation, tabular numerals, and muted metadata.
6. **Controls:** Compare icon size, radius, field height, chip treatment, active states, and alignment.
7. **Data:** Compare chart weight, gridlines, legend density, table alignment, map contrast, and status-color restraint.
8. **Finish:** Check clipping, overflow, focus, hover, loading, empty/error states, and responsive behavior in scope.

## Fidelity pass criteria

Pass only when:

- the screen silhouette remains recognizable when both images are mentally blurred
- every major region matches the reference's hierarchy and approximate proportion
- the UI shows comparable information density at the target viewport
- surface steps are subtle and borders carry most of the structure
- typography is compact and no heading dominates beyond the reference
- only one product accent is prominent and semantic colors stay sparse
- controls share one radius, icon, spacing, and state system
- there is no accidental horizontal scroll, clipped action, or unreadable text
- all in-scope interactions remain functional

## Repair strategy

- Fix a shared layout or token primitive when a mismatch repeats across the screen.
- Fix macro structure before adjusting individual components.
- Change one class of mismatch per pass so the next screenshot reveals causality.
- Prefer reducing padding, radius, shadow, and saturation when the output feels generic or soft.
- Prefer stronger alignment, clearer separators, and more realistic data when the output feels unfinished.
- If a component library fights the reference, override its tokens or rebuild the small primitive; do not accept the default appearance.

Do not hide remaining visual mismatches behind a successful build or test result. If a live screenshot cannot be captured, label the visual result unverified.
