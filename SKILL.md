---
name: research-plotting
description: Create or revise diagnostic scientific plots for research inspection, including multi-panel geoscience maps, line diagnostics, and bar comparisons. Use when inspecting analysis results or comparing datasets, experiments, models, variables, or periods; not as a publication-production workflow.
---

# Research plotting

Expose intermediate and final analysis results so the researcher can inspect structures, compare diagnostics, and decide what deserves further investigation. Prioritize **scientific correctness → comparability → readability → simplicity → aesthetics**. Do not require a figure-level claim, publication narrative, or journal formatting.

## Working principles

1. **Identify the diagnostic task.** Establish the quantities, dimensions to expand into panels or encodings, intended comparisons, and coordinates, scales, and physical meanings that must remain consistent. Use existing analysis definitions; ask only when an unresolved choice would change the scientific interpretation.
2. **Plan before plotting.** Choose rows, columns, panel order, shared or independent axes and color scales, and legend/colorbar placement before implementation. Preserve calendar, lag, model, experiment, variable, or forcing order as appropriate. Prefer regular layouts that make repeated comparisons easy to scan.
3. **Preserve comparability.** Keep domains, limits, normalization, contour levels, units, and category meanings consistent within each intended comparison. Choose independent scales for a scientific reason, not to make every panel prominent; identify their scope clearly.
4. **Use functional text.** Keep panel titles to the identifiers that distinguish neighboring panels. Put quantities and units on axes/colorbars, category meanings in legends, and necessary interpretive notes in concise annotations. Keep metadata summaries out of titles and subtitles.
5. **Encode the quantity.** Choose colormaps, normalization, contours, vectors, markers, reference lines, and uncertainty displays from the quantity's sign, physical/statistical meaning, dimensions, and whether magnitude or pattern is being compared. Preserve established semantics within related figures.
6. **Preserve the calculation.** Do not smooth, clip, rescale, normalize, mask, interpolate, filter, or otherwise transform scientific data merely for appearance. Follow scientifically required or established workflow operations, with truthful units and labels. Rendering-only subsampling, such as sparse vectors, may leave the underlying calculation unchanged.
7. **Keep implementation direct.** Reuse a nearby plotting pattern when it fits. Avoid decorative elements, generalized plotting frameworks, complex layout machinery, or publication infrastructure without an actual need. Follow `AGENTS.md`; change only the requested plotting work.
8. **Check proportionally, then stop.** Check panel/data identity, coordinates and order, domain, normalization and units, legend/colorbar associations, missing or clipped data, spatial seams, and comparison consistency as relevant. Inspect the resulting figure at a useful size. Do not introduce heavy automated visual QA or rerun the analysis by default.

## Load only the relevant guidance

| Diagnostic | Read |
|---|---|
| Geographic scalar fields, spatial panel comparisons, contours, winds, or map significance overlays | [Multi-panel spatial maps](references/multi-panel-spatial-maps.md) |
| Time series, seasonal cycles, lead–lag curves, or line comparisons | [Line plots](references/line-plots.md) |
| Grouped categories, period/dataset/model estimates, fractions, or bars with uncertainty | [Bar plots](references/bar-plots.md) |

For a mixed figure, read the relevant combination. Each reference links to one repository-derived example; load it when concrete plotting logic is useful. Examples assume already computed results and illustrate decisions rather than fixed templates.

The references were derived from `ENSO_IOD_INTERDECADAL`. Their source links provide provenance, not runtime dependencies. Relevant conventions in the current repository take precedence over illustrative settings here, subject to the user's instructions and scientific correctness. Keep the three reference categories fixed unless a clear recurring need cannot reasonably fit them.
