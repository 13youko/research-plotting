# Multi-panel spatial maps

Use for comparing geographic fields across periods, seasons, datasets, experiments, models, or variables. Start with the diagnostic comparison, then choose the layout and scale groups. See the [SLP and wind example](examples/multi-panel-spatial-map-example.md) for a compact extraction of an existing plot.

## Repository basis

Paths below refer to `ENSO_IOD_INTERDECADAL`; function names identify the relevant plotting portions.

| Source | Useful pattern |
|---|---|
| [WSW atmospheric bridge](</Volumes/T7 Shield/Pycharm_projects/ENSO_IOD_INTERDECADAL/weak-strong-weak/scripts/04_atmospheric_bridge_wsw.py>), `plot_regression_figure`, `add_wsw_quiver` | Six period/difference rows × eight event seasons; SLP/OLR/chi shading with wind vectors; two scale groups |
| [WSW basic features](</Volumes/T7 Shield/Pycharm_projects/ENSO_IOD_INTERDECADAL/weak-strong-weak/scripts/09_basic_enso_iod_features_wsw.py>), `plot_sst_evolution`, `setup_map_axis` | SST over a gray land backdrop, concise row/column labels, different palettes for period and difference groups |
| [CMIP6 comparisons](</Volumes/T7 Shield/Pycharm_projects/ENSO_IOD_INTERDECADAL/PeriodFeatureAnalysis/scripts/cmip6/03_plot_cmip6_strong_weak_diff.py>), `plot_lonlat_with_diff`; [shared helpers](</Volumes/T7 Shield/Pycharm_projects/ENSO_IOD_INTERDECADAL/PeriodFeatureAnalysis/scripts/lib/cmip6_composite_tools.py>), `setup_map_axis`, `get_contour_levels_from_values` | Strong/Weak/Strong−Weak grids; positive standard deviations versus signed differences; scalar wind contours |
| [Walker circulation](</Volumes/T7 Shield/Pycharm_projects/ENSO_IOD_INTERDECADAL/MechanismDiagnose/HadISST1940-2014/scripts/03b_walker_circulation_maps.py>), `plot_seasonal_level` | A single shared scale across strong, weak, and their difference; explicit chi display units |
| [Background state](</Volumes/T7 Shield/Pycharm_projects/ENSO_IOD_INTERDECADAL/weak-strong-weak/scripts/11_background_state_wsw.py>), `plot_sst_wind_maps`, `add_wind_quiver` | Raw SST versus differences; separate raw/difference wind scales with matching quiver keys |
| [Confidence overlays](</Volumes/T7 Shield/Pycharm_projects/ENSO_IOD_INTERDECADAL/MechanismDiagnose/HadISST1940-2014/scripts/07_plot_confidence_overlays.py>), `map_mask`, `add_lonlat_stippling`, `plot_atmospheric_regression` | Coordinate-matched significance stippling on the tested difference field, retaining the scalar field underneath |
| [Per-model index diagnostics](</Volumes/T7 Shield/Pycharm_projects/ENSO_IOD_INTERDECADAL/piControl-strong-weak/scripts/01b_plot_index_diagnostics.py>), `plot_diagnostic` | Pacific ENSO and Indian Ocean DMI maps with different domains/projection centers and individual colorbars |

## Panel design and labels

- Periods commonly occupy rows; event seasons or relative months occupy columns. Preserve the analysis-defined order explicitly. In the atmospheric example the rows are `W1, S, W2, W1-W2, S-W1, S-W2`; the SST example orders differences differently. Select by labels or a documented array order, not assumptions based on a neighboring script.
- Event-season columns run from `MAM(0)` through `DJF(+1/+2)`. Calendar-month climatology and relative-to-peak-month plots have different origins. Retain year offsets and the actual month/season coordinates; do not alphabetize season names.
- Use `plt.subplots(..., squeeze=False)` for a regular grid. Put season titles on the top row, period labels along the first column, longitude labels on the bottom row, and latitude labels on the first column when domains are shared. Repeated titles need not restate the variable or dataset.
- Reserve colorbar and quiver-key space before plotting. The repository often uses `subplots_adjust` and one or two explicit colorbar axes. Reuse those simple arrangements when they fit; exact figure sizes and spacing constants are not conventions. Check the map aspect ratio at the actual viewing size.

## Coordinates, projection, and land

- The common Indo-Pacific view uses `ccrs.PlateCarree(central_longitude=180)` for the **axes**, with `transform=ccrs.PlateCarree()` on geographic fields, contours, and vectors. Apply the data CRS to `set_extent` and geographic tick positions too. Centering the display does not change the data's longitude values.
- Typical domains are 30–290°E, 30°S–30°N in the atmospheric/Walker work, and 40–290°E in the WSW SST evolution. Use the domain of the actual diagnostic and keep it fixed across comparable panels. Per-model ENSO/DMI diagnostics deliberately use separate Pacific and Indian Ocean domains (including an 80° center for DMI); do not force them into the full Indo-Pacific view.
- Existing analysis helpers prepare longitudes in 0–360 order before plotting. Honor the supplied coordinate convention and latitude/longitude dimension order. If coordinate wrapping is necessary, reorder every associated field and overlay with the coordinates; never relabel a longitude array without moving the data. Keep regional edges distinct from a global periodic boundary.
- The inspected regional plotting functions do not establish an automatic cyclic-point step. Inspect seams if they appear, especially after changing domain/projection or using a global grid. Diagnose coordinate ordering and coverage before adding any wrap column; never interpolate, smear, or mask a seam just to hide it.
- Thin coastlines recur. Ocean-only SST maps commonly cover land in light gray; atmospheric maps often show the scalar field over both land and sea with coastlines only. Match the quantity and the existing scientific mask. A land backdrop is not permission to replace a scientific land/sea mask or erase meaningful land values.
- Longitude labels use E/W notation while coordinates remain 0–360. Latitude labels use either signed degrees or N/S notation. Keep each figure consistent and avoid overcrowding; copy tick locations from the relevant domain, not from a map with another extent.

## Fields, normalization, and colorbars

`pcolormesh(..., shading="auto")` is the dominant field renderer in the inspected scripts. Use the native supplied grid and missing values. If adapting an existing `contourf` figure, retain scientifically meaningful level boundaries and use the same levels across its comparison group; do not replace the renderer for decoration.

| Quantity/comparison | Repository-derived decision |
|---|---|
| Signed anomalies, regressions, differences | Zero-centered symmetric limits; `RdBu_r` is common |
| Nonnegative standard deviation | Zero lower bound and positive upper bound; CMIP6 composites use `YlOrRd` |
| Difference of standard deviations | A signed field despite its nonnegative inputs; use a diverging scale |
| Period fields and a separately inspected difference group | Pool limits over all panels within each group; use separately labeled colorbars. SST examples use `RdBu_r` for period fields and `PuOr_r` for signed differences |
| Absolute magnitudes compared across period and difference rows | Share the scale across all those rows; Walker seasonal maps demonstrate this alternative |
| Raw SST background versus SST differences | Raw-field range and signed-difference range have different meanings. The background-state script uses `turbo` for raw SST; that palette is local to this workflow, not a default for every unsigned field |

Determine limits before the panel loop. Across related figures, reuse an established common scale when they are intended for comparison; independent per-model autoscaling permits pattern inspection but does not support direct color-magnitude comparisons.

Several helpers use finite extrema; WSW atmospheric regressions use the 98th percentile of absolute values pooled within each group. Preserve percentile limits only when they are part of the relevant diagnostic convention. They saturate displayed tails and can obscure extremes: keep values unchanged, identify the scale choice in a short note or accompanying output, and mark out-of-range colors with appropriate colorbar extensions. Do not silently introduce percentile limits for a new diagnostic or use `clip`/winsorization on the data.

Use a mappable from the correct scale group for each shared colorbar, with the quantity and units (including the regression denominator). Shared colors do not make raw-index and standardized-index regressions equivalent. Walker chi is displayed in `10^6 m² s⁻¹`; retain that explicit unit convention where used, not arbitrary amplitude rescaling. A colorbar must not silently describe panels with other limits or units.

## Overlays

**Scalar contours:** CMIP6 SST/UAS plots use thin black UAS contours, shared levels within period/difference groups, and small inline numeric labels. The 0.2 contour step is a UAS-specific local convention, not a generic step for other units. Positive-only levels suit UAS standard deviation; signed UAS regressions/differences use signed levels, with zero often omitted. Identify the contour variable and units separately from the shading colorbar.

**Wind vectors:** Sample longitude, latitude, U, and V using the same stride; this reduces glyph density, not the analyzed field resolution. Use the components already paired and aligned by the workflow; do not invent interpolation between grids. Match arrows to the same period, season, regression predictor, and difference order as the shaded panel. Reuse a fixed scale and `scale_units` within each vector comparison group. Atmospheric regression uses one fixed vector scale; raw/background versus difference winds may have separate scales, which the background-state script labels with separate quiver keys. Use a key when magnitude matters; do not rescale each panel independently without disclosure.

**Significance:** Overlay the existing confidence result on precisely the field/contrast it tests. The confidence script matches diagnostic family, variable, regressor, season, and grid coordinates, and adds black stippling to the strong-minus-weak row while retaining nonsignificant scalar values. Keep raw p-value and FDR-adjusted results distinct and label the actual criterion. Do not silently substitute one for the other, infer a difference test from individual significance, or treat an unavailable test as nonsignificance. Sparse stippling may aid rendering; it does not change the full test mask. Avoid combining contours, vectors, and stippling unless each answers the diagnostic question readably.

## Related longitude–month fields

[Longitude–month diagnostics](</Volumes/T7 Shield/Pycharm_projects/ENSO_IOD_INTERDECADAL/MechanismDiagnose/HadISST1940-2014/scripts/05_lonmonth_regression_diagnostics.py>), `plot_field_for_band`, reuse the scalar shading/contour and grouped-colorbar decisions above on ordinary Matplotlib axes: longitude versus event-month offset, shared coordinates, and an explicit latitude-band identifier. Preserve the established month direction and offset labels. These are not geographic maps: do not add Cartopy transforms, coastlines, or latitude ticks. This related case fits here without another plot-type reference.

## Check the result

Compare a few known field values/locations with the intended panels, including a difference sign. Check domain and coordinate alignment, all group limits and units, the colorbar-to-mappable association, overlay units/scale/mask identity, NaNs versus legitimate zeros, unintended saturation, and longitude seams. View the saved image for unreadable labels or overlays and clipped bars/keys. Fix the concrete problem and stop; no visual regression framework is needed.
