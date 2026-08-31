# Example: a wind–ocean dataset comparison with intervals

## Provenance and purpose

Based on `plot` in [09c_plot_multidata_response.py](</Volumes/T7 Shield/Pycharm_projects/ENSO_IOD_INTERDECADAL/weak-strong-weak/scripts/09c_plot_multidata_response.py>). The existing [2×3 response figure](</Volumes/T7 Shield/Pycharm_projects/ENSO_IOD_INTERDECADAL/weak-strong-weak/figures/06_summary/multidata_1958_1971/jja_cio_uas_eio_d20_significance_2x3_1958_1971.png>) was visually inspected: paired period estimates remain directly comparable across all six panels, including their confidence intervals.

The scientific quantity is the regression of JJA eastern Indian Ocean D20 on central Indian Ocean surface zonal wind. Ocean datasets occupy rows, wind datasets occupy columns, and gray/orange bars distinguish 1958–1971 from 1972–1998. The existing table supplies slopes and 90% two-sided OLS t-test confidence intervals.

## Already computed input

`table` is the validated source table, with exactly one record per `(ocean_dataset, wind_dataset, period_label)` combination. Required columns are those three identifiers plus `slope`, `ci_lower_90`, and `ci_upper_90`. The two period labels below use the source's en dash. Slope units are m D20 per m s⁻¹ surface zonal wind. Intervals bracket their corresponding estimates.

This extraction retains the source ordering, colors, paired positions, zero baseline, CI-distance calculation, and shared range. It omits data loading/validation, cross-dataset correlations, p-value annotations, the metadata-heavy heading, and filesystem operations. The interval definition is kept as a concise figure note.

```python
import matplotlib.pyplot as plt
from matplotlib.patches import Patch
import numpy as np

oceans = ("IAPv4", "ORAS5")
winds = ("ERA5", "JRA-55", "20CRv3")
periods = ("1958–1971", "1972–1998")
colors = {"1958–1971": "#9E9E9E", "1972–1998": "#F28E2B"}
records = table.set_index(["ocean_dataset", "wind_dataset", "period_label"])
# These already validated intervals bracket all estimates.
y_limit = 1.28 * np.max(np.abs(table[["ci_lower_90", "ci_upper_90"]].to_numpy()))
fig, axes = plt.subplots(2, 3, figsize=(14.8, 7.8), sharey=True, squeeze=False)
fig.subplots_adjust(
    left=0.105, right=0.985, top=0.82, bottom=0.10,
    hspace=0.20, wspace=0.20,
)
for i, ocean in enumerate(oceans):
    for j, wind in enumerate(winds):
        ax = axes[i, j]
        for x, period in zip((-0.22, 0.22), periods):
            record = records.loc[(ocean, wind, period)]
            estimate = float(record["slope"])
            lower = float(record["ci_lower_90"])
            upper = float(record["ci_upper_90"])
            ax.bar(
                x, estimate, width=0.34, color=colors[period],
                edgecolor="0.20", linewidth=0.65,
                yerr=np.array([[estimate - lower], [upper - estimate]]),
                error_kw={"ecolor": "0.12", "elinewidth": 1.15, "capsize": 4},
                zorder=3,
            )
        ax.axhline(0, color="0.30", linewidth=0.9, zorder=1)
        ax.set_axisbelow(True)
        ax.grid(axis="y", linestyle=":", linewidth=0.55, alpha=0.55)
        ax.set_xlim(-0.65, 0.65)
        ax.set_ylim(-y_limit, y_limit)
        ax.set_xticks([])
        if i == 0:
            ax.set_title(wind, fontsize=12)
        if j == 0:
            ax.text(
                -0.14, 0.5, ocean, transform=ax.transAxes,
                rotation=90, va="center", ha="center", fontsize=12,
            )
fig.supylabel("D20 response (m per m s⁻¹ surface zonal wind)", x=0.018)
fig.suptitle("JJA CIO wind–EIO D20 regression", fontsize=14)
fig.legend(
    handles=[Patch(facecolor=colors[p], edgecolor="0.2", label=p) for p in periods],
    loc="upper center", bbox_to_anchor=(0.5, 0.94), ncol=2, frameon=False,
)
fig.text(
    0.5, 0.03, "Error bars: 90% two-sided OLS t-test confidence intervals",
    ha="center", fontsize=10,
)
```

The reusable logic is that the panel grid expands the two dataset dimensions, bar positions/colors identify periods, and one physical scale covers every estimate and interval. Choose different domains or independent y-scales only when the compared quantity warrants them; do not use independent limits to amplify each panel.

For multiple discrete categories within one axis, use centered grouped offsets from the [bar reference](../bar-plots.md). Neither two period bars nor a symmetric y-range is required for every bar diagnostic. Do not infer a between-period significance test from these individual confidence intervals.

Check one record against its ocean/wind/period panel, recover its plotted interval endpoints from the error distances, and verify that all endpoints fit the shared limits. Confirm period colors, units, and legend association. Save/display `fig` with the task's established workflow.
