# Example: seasonal SLP regression with surface winds

## Provenance and purpose

Based on `plot_regression_figure` and `add_wsw_quiver` in [04_atmospheric_bridge_wsw.py](</Volumes/T7 Shield/Pycharm_projects/ENSO_IOD_INTERDECADAL/weak-strong-weak/scripts/04_atmospheric_bridge_wsw.py>). The existing [SLP/wind figure](</Volumes/T7 Shield/Pycharm_projects/ENSO_IOD_INTERDECADAL/weak-strong-weak/figures/02_atmospheric_bridge/atmospheric_bridge_slp_u10v10_nino34_regression_wsw.png>) was visually inspected: its regular rows and seasons make circulation evolution and period differences easy to scan.

The diagnostic is ERA5 SLP shading plus u10/v10 vectors, regressed onto the raw event-year DJF Niño3.4 index. Rows contain W1/S/W2 followed by three signed contrasts. Columns preserve eight event seasons. Separate shared color scales expose period fields and contrasts; vector scale is fixed across both groups.

The extraction below omits loading, regression, differencing, output handling, and batch logic. It keeps the source's 98th-percentile color limits, adds explicit saturation disclosure, and adds a physical vector key using the pattern in `plot_sst_wind_maps` in [11_background_state_wsw.py](</Volumes/T7 Shield/Pycharm_projects/ENSO_IOD_INTERDECADAL/weak-strong-weak/scripts/11_background_state_wsw.py>). This is plotting-only adaptation, not a verbatim source function or a new analysis.

## Already computed inputs

- `slp`, `u10`, `v10`: arrays shaped `(6, 8, nlat, nlon)`, in the row and season order below. Difference fields are already computed as first named period minus second for every component.
- SLP units: Pa per °C DJF Niño3.4; vector units: m s⁻¹ per °C DJF Niño3.4. These are regression coefficients, not raw winds.
- `lon`, `lat`: the common one-dimensional coordinates of those fields, in data order; this diagnostic uses the 30–290°E, 30°S–30°N region. Keep source NaNs. Each color-scale group has finite, nonzero data.

```python
import cartopy.crs as ccrs
import matplotlib.pyplot as plt
import numpy as np

rows = ("W1", "S", "W2", "W1-W2", "S-W1", "S-W2")
seasons = (
    "MAM(0)", "JJA(0)", "SON(0)", "DJF(0/1)",
    "MAM(+1)", "JJA(+1)", "SON(+1)", "DJF(+1/+2)",
)
# Existing diagnostic convention: pooled display limits, not clipped data.
main_limit = np.nanpercentile(np.abs(slp[:3]), 98)
diff_limit = np.nanpercentile(np.abs(slp[3:]), 98)
data_crs = ccrs.PlateCarree()
fig, axes = plt.subplots(
    6, 8, figsize=(27.5, 8.0), squeeze=False,
    subplot_kw={"projection": ccrs.PlateCarree(central_longitude=180)},
)
fig.subplots_adjust(
    left=0.05, right=0.995, top=0.86, bottom=0.17,
    hspace=0.02, wspace=0.055,
)
group_images = {}
for i, row in enumerate(rows):
    group = "Periods" if i < 3 else "Differences"
    limit = main_limit if i < 3 else diff_limit
    for j, season in enumerate(seasons):
        ax = axes[i, j]
        ax.set_extent((30, 290, -30, 30), crs=data_crs)
        ax.coastlines(linewidth=0.45)
        im = ax.pcolormesh(
            lon, lat, slp[i, j], transform=data_crs,
            cmap="RdBu_r", vmin=-limit, vmax=limit, shading="auto",
        )
        group_images[group] = im
        q = ax.quiver(
            lon[::12], lat[::12], u10[i, j, ::12, ::12],
            v10[i, j, ::12, ::12], transform=data_crs,
            color="0.12", width=0.0028, headwidth=4.2,
            headlength=5.0, headaxislength=4.6, alpha=0.82,
            scale=9.0, scale_units="inches",
        )
        if i == 5:
            ax.set_xticks([30, 80, 130, 180, 230, 280], crs=data_crs)
            ax.set_xticklabels(["30E", "80E", "130E", "180", "130W", "80W"])
        if j == 0:
            ax.set_yticks([-30, -15, 0, 15, 30], crs=data_crs)
            ax.set_yticklabels(["30S", "15S", "0", "15N", "30N"])
            ax.text(
                -0.19, 0.5, row, transform=ax.transAxes,
                rotation=90, va="center", ha="center", fontsize=9,
            )
        if i == 0:
            ax.set_title(season, fontsize=8.5)
        ax.tick_params(labelsize=8, length=2)

axes[-1, -1].quiverkey(
    q, 0.68, 0.945, 1.0, "1 m s⁻¹ per °C Niño3.4",
    coordinates="figure", labelpos="E",
)
for group, left in (("Periods", 0.16), ("Differences", 0.57)):
    cax = fig.add_axes([left, 0.065, 0.30, 0.026])
    cb = fig.colorbar(
        group_images[group], cax=cax, orientation="horizontal", extend="both",
    )
    cb.set_label(f"{group}: SLP regression (Pa per °C Niño3.4)")
fig.suptitle("SLP and surface-wind regression onto DJF Niño3.4", fontsize=12)
fig.text(
    0.5, 0.015, "Color limits: pooled 98th percentile of |SLP regression| in each group",
    ha="center", fontsize=9,
)
```

Keep one scalar limit across all period and difference rows instead if cross-group absolute magnitude is the intended comparison. This example's two ranges and percentile choice belong to the source diagnostic; they are not defaults for a new quantity. The vector stride and scale likewise depend on grid density and physical units.

This atmospheric example deliberately does not cover land in gray or add significance markers. For SST, scalar wind contours, or existing confidence fields, use the corresponding decisions in the [spatial reference](../multi-panel-spatial-maps.md), rather than overlaying everything at once.

Check at least one period/season selection and one contrast sign, that U/V describe the same regression as SLP, that both colorbar extensions correspond to saturated tails, and that the key uses regression units. Save/display `fig` through the task's existing workflow; inspect labels, vector density, and map seams.
