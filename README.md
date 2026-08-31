# Research Plotting Skill

A personal scientific plotting skill for diagnostic figures used during research.

The skill is designed for inspecting analysis results, comparing datasets, experiments, models, variables, and periods, and identifying spatial or temporal structures worth further investigation.

Its priority is:

**scientific correctness → comparability → readability → simplicity → aesthetics**

It is intended for research-stage diagnostic plotting rather than publication-production workflows.

## Installation

For local use with Codex, clone this repository into the [user skills directory](https://learn.chatgpt.com/docs/build-skills#where-codex-loads-local-skills):

```sh
mkdir -p ~/.agents/skills
git clone https://github.com/13youko/research-plotting.git ~/.agents/skills/research-plotting
```

Then ask Codex to use `research-plotting` for a plotting task. If the skill does not appear, restart Codex.

## Structure

```text
research-plotting/
├── README.md
├── SKILL.md
└── references/
    ├── multi-panel-spatial-maps.md
    ├── line-plots.md
    ├── bar-plots.md
    └── examples/
        ├── multi-panel-spatial-map-example.md
        ├── line-plot-example.md
        └── bar-plot-example.md
```

`SKILL.md` and `references/` are for the agent. This README is repository documentation for people, not a runtime reference; plotting instructions are maintained in the skill files.

## Plotting references

The skill currently focuses on three recurring figure types:

- **[Multi-panel spatial maps](references/multi-panel-spatial-maps.md)** — geoscience maps using Cartopy, including projections, longitude handling, shared color scales, vector overlays, significance displays, and related spatial diagnostics.
- **[Line plots](references/line-plots.md)** — time series, seasonal cycles, lead–lag diagnostics, and experiment/model comparisons.
- **[Bar plots](references/bar-plots.md)** — grouped estimates, dataset/model comparisons, uncertainty intervals, and related categorical diagnostics.

The references were derived from plotting patterns used in an existing geoscience research workflow. The included examples illustrate reusable decisions rather than fixed plotting templates.

## Scope

This repository contains only the plotting skill, its references, and this overview. Scientific calculations, preprocessing, datasets, and project-specific analysis code remain outside this repository.
