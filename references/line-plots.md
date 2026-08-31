# Line plots

Use lines to inspect an ordered coordinate: time, calendar month, lag, or another scientifically ordered sequence. See the [lead–lag example](examples/line-plot-example.md). Do not connect unordered model/category estimates just because they can be assigned integer positions; use the bar reference when appropriate.

## Repository basis

Source paths below are relative to `ENSO_IOD_INTERDECADAL` and document provenance only; the referenced files are not required to use this skill.

| Source | Useful pattern |
|---|---|
| WSW basic features — `weak-strong-weak/scripts/09_basic_enso_iod_features_wsw.py`, `plot_lead_lag`, `plot_monthly_std` | Two lead–lag cases with common correlation limits; monthly ENSO/DMI seasonality with twin axes |
| HadISST features — `PeriodFeatureAnalysis/scripts/hadisst/02_plot_hadisst_full_period_features.py`, `plot_index_monthly_std`, `plot_rolling_correlation` | Index color versus period line style; actual year coordinate for a precomputed rolling diagnostic |
| Observational comparisons — `PeriodFeatureAnalysis/scripts/obs_compare/01_compare_era5_hadisst.py`, `plot_index_timeseries` | One index per row, shared time axis, stable dataset colors, explicitly standardized indices |
| Index diagnostics — `piControl-strong-weak/scripts/01b_plot_index_diagnostics.py`, `plot_diagnostic` | Monthly curves with markers on already identified peak months, alongside spatial diagnostics |
| Mechanism lead–lag — `MechanismDiagnose/HadISST1940-2014/scripts/02_enso_dmi_lead_lag_correlation.py`, `LINE_STYLES`, `plot_lead_lag`; confidence overlays — `MechanismDiagnose/HadISST1940-2014/scripts/07_plot_confidence_overlays.py`, `plot_lead_lag` | Distinct period/all-time/difference lines; test markers attached to the tested difference curve |

## Coordinate and panel decisions

- **Time series:** Use actual dates or the analysis's year coordinate. Overlay datasets for the same quantity; use rows for different indices and share the time axis. Keep gaps visible rather than filling them or deleting missing samples so a line bridges them. Preserve any established rolling window, filtering, or standardization as part of the supplied scientific diagnostic; do not introduce these in order to smooth the plot.
- **Seasonal cycles:** Calendar month is ordered 1–12, Jan–Dec. Single-letter labels recur in compact figures; three-letter labels also exist. Tick positions, values, and labels must agree. An event-relative or peak-relative axis is a different coordinate and needs its own offset labels. Peak markers denote the peak already identified by the analysis, not an independently selected plotting peak.
- **Lead–lag:** Put distinct anchor/response definitions in separate panels. The WSW example uses −24…+24 months, common `[-1, 1]` correlation limits, and reference lines at zero lag and zero correlation. Positive lag means the responding monthly index lags the anchored seasonal index; the named indices switch between its two panels. State that direction near each axis and preserve the anchor month from the calculation. Never infer the lag sign from a curve's appearance.
- **Multiple experiments/models:** Use an explicit identity order and mapping for comparable curves. Share coordinate ranges and y-limits where magnitude is compared. Keep ensemble/member selection and any aggregate definition from the analysis; do not average curves in the plotting layer to simplify a crowded figure. Use regular panels when overlays become unreadable.

## Scales and visual semantics

Use the same y-scale for the same quantity when comparing magnitudes. `sharex` and `sharey` need not be coupled: time-series rows can share time without implying identical physical quantities. Pearson correlation lies in `[-1, 1]`, but a **difference of correlations** can exceed those bounds; do not blindly copy the absolute-correlation limits for a difference curve or its interval.

Twin axes recur for monthly ENSO and DMI standard deviations: each index keeps its physical units, the left/right labels and tick colors identify the index, and period line styles are consistent across both axes. This arrangement supports inspecting seasonal timing and within-index period changes; visual line heights across the two axes are not directly comparable amplitudes. If cross-index amplitudes are the task, use a common physical scale or separately labeled panels. Do not normalize each curve merely to align peaks.

Local color meanings differ by task:

- WSW lead–lag: W1 blue, S red, W2 green; one consistent mapping across both panels.
- Monthly ENSO/DMI std: index identity uses blue/red, while W1/S/W2 use dashed/solid/dash-dot styles.
- Mechanism strong/weak/all-time/difference: red solid, blue dashed, black solid, and purple dotted lines respectively.

Preserve the relevant mapping across related figures, without imposing one global meaning on every color. Use modest line widths and light grids/reference lines so curves remain readable. Markers add a role such as selected month or a tested lag; they are not required on every point.

## Labels, legends, and uncertainty

Use short index/case titles, physical units on y-axes, and time/month/lag definitions on x-axes. A short lag-direction note is interpretation, not a metadata dump. An optional figure title can identify the common diagnostic. Keep full period definitions and calculation details in the analysis output when they are already established.

Use one legend for a shared mapping where practical. Combine line handles from both axes for a twin-axis figure; do not let one index disappear from the legend. Check that a legend inside an axis does not obscure relevant peaks or crossings.

The basic WSW lead–lag workflow explicitly computes no uncertainty; do not add inferred confidence bands. The separate confidence workflow uses black markers on significant strong-minus-weak lags and identifies its Fisher-z criterion. Use only available tests/intervals, preserving their confidence level and tested quantity. If the task supplies uncertainty bands, align their coordinates and color association with the corresponding curve and include their bounds in the visible range; the inspected examples do not establish a universal band style.

## Check the result

Verify curve identity and ordering; calendar versus event-relative coordinates; lag sign and anchor; common comparison limits; units or explicitly established z-scores; preserved gaps; reference-line meaning; legend handles; and any tested-lag or uncertainty association. Inspect the figure for obscured curves or clipped extrema. Reuse the existing save/display pattern and stop once it is correct and readable.
