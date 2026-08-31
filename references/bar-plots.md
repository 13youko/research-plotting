# Bar plots

Use bars for discrete estimates, counts, fractions, and period/dataset/model comparisons. Plan what each panel, bar group, bar color, and reference line means before plotting. See the [dataset-comparison example](examples/bar-plot-example.md).

## Repository basis

| Source | Useful pattern |
|---|---|
| [Index geometry](</Volumes/T7 Shield/Pycharm_projects/ENSO_IOD_INTERDECADAL/weak-strong-weak/scripts/03_index_geometry_wsw.py>), `grouped_bar_panel`, `plot_overview` | Fixed W1/S/W2 order; centered groups of index estimates; units and ranges appropriate to each metric |
| [Coupling strength](</Volumes/T7 Shield/Pycharm_projects/ENSO_IOD_INTERDECADAL/weak-strong-weak/scripts/02_coupling_strength_wsw.py>), `plot_metric_bars`, `set_bar_limits` | Period colors, zero baseline, full correlation domain versus data-derived metric ranges |
| [Multiple datasets](</Volumes/T7 Shield/Pycharm_projects/ENSO_IOD_INTERDECADAL/weak-strong-weak/scripts/09c_plot_multidata_response.py>), `plot` | Ocean dataset × wind dataset grid; paired period bars; shared y-range including existing confidence endpoints |
| [Mechanism intervals](</Volumes/T7 Shield/Pycharm_projects/ENSO_IOD_INTERDECADAL/weak-strong-weak/scripts/08b_revised_mechanism_ols_ttest_ci_wsw.py>), `plot_ci_figure` | Grouped seasonal regression estimates; explicit lower/upper CI distances; separate relation units |
| [IPO background](</Volumes/T7 Shield/Pycharm_projects/ENSO_IOD_INTERDECADAL/piControl-strong-weak/scripts/05d_plot_ipo_background.py>), `plot_group_summary`, `plot_episode_mean_tpi` | Fractions with count/denominator labels; one model per panel; chronological episodes and a pooled strong-period reference |
| [IPO/AMO associations](</Volumes/T7 Shield/Pycharm_projects/ENSO_IOD_INTERDECADAL/piControl-strong-weak/scripts/05k_plot_ipo_amo_coupling_associations.py>), `plot_group_summary`, `plot_conditional_proportions` | Fixed model order, probability axes, individual-model points with equal-model mean bars, explicitly different conditional probabilities |

## Ordering and grouping

Select values by the established identifiers before assigning positions. W1/S/W2, season, and dataset orders are explicit in the source scripts. Keep the same model/member order across panels; episode bars use chronological start order, with a separately identified pooled S reference. Do not silently sort each panel by magnitude or substitute alphabetical order.

For `k` bars per category, the index-geometry code centers offsets using `(arange(k) - (k - 1) / 2) * width`; category ticks stay at group centers. Leave space between groups. For just two period estimates per panel, the dataset example uses positions −0.22 and +0.22 and a shared period legend instead of repeating x-labels. Exact widths and offsets are illustrative.

Choose what belongs in rows/columns according to the comparison: ocean datasets in rows and wind datasets in columns expose dataset dependence; one model per panel exposes episode structure. Use colors consistently for the same categories. The dataset example uses gray/orange for its two periods; WSW period colors elsewhere are blue/red/green. Retain the local mapping rather than imposing a global palette.

## Axes, values, and units

- Keep a visible zero baseline. Signed regression bars extend on either side of zero; negative values must not be turned into magnitudes. Standard deviations, counts, and fractions keep their nonnegative interpretation.
- Share y-limits across panels representing the same quantity and units when magnitude is compared. The six dataset panels share one CI-inclusive range. Mechanism-chain panels have different response/predictor units, so a single numerical y-scale would not necessarily support comparison.
- Correlation bars commonly use `[-1, 1]`. Difference statistics are not automatically confined to the original statistic's bounds. Fraction/probability panels show 0–1; small extra space for count labels, as in the IPO summary, does not imply a changed denominator or values over one.
- Put the full coefficient unit on the y-axis, including the predictor denominator. Keep counts, fractions, conditional probabilities, pooled estimates, and equal-model means distinct. In particular, `P(negative phase | state)` and `P(remote-W | phase, S/W only)` are different diagnostics even though both use 0–1 axes.
- Use values already computed with the established weighting. Do not recompute a model mean or pool episodes differently to simplify plotting. Model/member selection, failed cases, and missing estimates must remain visible; a missing estimate is not a zero-height bar.

## Uncertainty and reference values

Use the supplied interval and level. The response comparison draws existing 90% two-sided OLS t-test CIs; related mechanism plots also supply interval endpoints. Matplotlib `yerr` takes **distances from the estimate**, not absolute endpoints: `[[estimate - lower], [upper - estimate]]`. Do not force an asymmetric interval to be symmetric, take absolute values to hide invalid bounds, or replace a CI with standard deviation/standard error. If the interval cannot be represented by that bar/error form, flag the mismatch and choose a faithful interval display.

Derive a shared visible range from every estimate and interval endpoint in the comparison group, including zero, then leave enough room for any annotations. Use restrained caps and contrasting error lines. Identify the interval method/level in a short note or legend, rather than embedding the entire analysis description in a title. CI overlap is not a supplied test of the difference between two bars.

Pooled-reference lines belong to the model/quantity whose reference they represent. Preserve the analysis's weighting and units, and distinguish them from zero. Where individual-model points accompany an aggregate bar, label both meanings; their spread is not automatically a confidence interval. Rendering-only horizontal jitter may separate categorical points without changing their y-values or category membership.

## Text and verification

Use short panel identifiers, category ticks at the group centers, one legend for a shared mapping, and y-only light grids where helpful. Rotate model labels only enough to read them. Numerical labels are optional: retain useful count/denominator information or a selected estimate, but omit dense per-bar metadata when it distracts from the comparison. Place labels beyond the relevant bar/interval endpoint and check that they fit.

Verify the selected record in each bar, category order, group centering, sign, baseline, units, shared ranges, interval endpoints, legend colors, denominators, and pooled/model weighting already used by the analysis. Inspect for missing estimates presented as zero and clipped error bars or annotations. No heavy visual-QA pipeline is needed.
