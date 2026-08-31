# Example: two ENSO–DMI lead–lag diagnostics

## Provenance and purpose

Derived from the `ENSO_IOD_INTERDECADAL` workflow: `weak-strong-weak/scripts/09_basic_enso_iod_features_wsw.py`, function `plot_lead_lag` and definitions `DIAGNOSTIC_CASES` and `LEAD_LAG_LINE_STYLES`. This example was selected because its comparable panels preserve distinct anchor definitions and consistent W1/S/W2 curves.

The first panel correlates DJF Niño3.4 with monthly DMI; the second correlates SON DMI with monthly Niño3.4. In each, positive lag places the monthly response after the seasonal anchor. The scientific calculation anchors these at Jan(+1) and Oct(0), respectively. Do not recompute or shift correlations while plotting.

## Already computed inputs

- `lead_lag`: Pearson correlations shaped `(2, 3, 49)`, with cases in the order above, periods `W1, S, W2`, and lags −24 through +24 months.
- Missing correlations remain NaN. This basic workflow has no supplied confidence intervals or significance tests.

The plotting-only adaptation shortens panel titles and moves positive-lag interpretation to the x-axis label. Colors, panel comparison, physical lag coordinate, and correlation bounds follow the source.

```python
import matplotlib.pyplot as plt
import numpy as np

lags = np.arange(-24, 25)
periods = ("W1", "S", "W2")
styles = {
    "W1": {"color": "tab:blue", "linewidth": 1.8},
    "S": {"color": "tab:red", "linewidth": 1.9},
    "W2": {"color": "tab:green", "linewidth": 1.8},
}
cases = (
    ("DJF Niño3.4 vs monthly DMI", "+lag: DMI lags Niño3.4"),
    ("SON DMI vs monthly Niño3.4", "+lag: Niño3.4 lags DMI"),
)
fig, axes = plt.subplots(1, 2, figsize=(13.2, 4.8), sharey=True)
for case_index, (ax, (title, lag_note)) in enumerate(zip(axes, cases)):
    for period_index, period in enumerate(periods):
        ax.plot(
            lags, lead_lag[case_index, period_index],
            label=period, linestyle="-", **styles[period],
        )
    ax.axvline(0, color="0.4", linewidth=0.9, linestyle="--")
    ax.axhline(0, color="0.65", linewidth=0.8, linestyle=":")
    ax.set_title(title, fontsize=10.5)
    ax.set_xlabel(f"Lag (months)\n{lag_note}")
    ax.set_xlim(-24, 24)
    ax.set_ylim(-1, 1)
    ax.grid(True, linestyle=":", linewidth=0.6, alpha=0.65)
axes[0].set_ylabel("Pearson correlation coefficient")
axes[1].legend(frameon=False, loc="lower right", fontsize=8)
# Optional common figure title when needed for identification.
# fig.suptitle("HadISST W1/S/W2 lead–lag correlation", fontsize=13)
fig.tight_layout()
```

The reusable logic is consistent period identity, a separate panel for each anchor/response definition, shared correlation limits, and reference lines whose meaning is clear. The 24-month range and WSW palette belong to this diagnostic.

Do not reuse `[-1, 1]` blindly for a difference curve or another quantity, or invent confidence bands from these curves. For a calendar-month seasonal cycle, use the distinct axis and optional twin-axis decisions in the [line reference](../line-plots.md).

Check a known correlation at its actual lag, both case orders, the two positive-lag definitions, any NaN gaps, the shared scale, and legend visibility. Save/display `fig` with the established workflow; no data loading or scientific calculation is included here.
