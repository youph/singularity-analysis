# World Population Singularity Analysis

## Research Question
This project revisits the classic "population singularity" hypothesis and asks:

- Did global population growth historically follow a hyperbolic power-law trend with a finite-time singularity?
- If so, when did the inferred singularity year begin to move or destabilize as new data became available?
- Does rolling Bayesian inference indicate a regime shift away from singular behavior?

In practical terms, we estimate the hyperbolic model

$$
N(t) = \frac{C}{(t_0 - t)^\alpha}
$$

across rolling inference windows and track how the inferred parameters (especially $t_0$) evolve over time.

## Approach Summary
- Data source: Our World in Data (OWID) population series (world aggregate).
- Model: Bayesian hyperbolic growth model with constrained priors.
- Inference protocol: rolling-window estimation over historical cutoffs.
- Diagnostics: training fit quality and reliability gating.
- Visualization: stability dashboard for $t_0$, $\alpha$, and MAPE.

## Stability Analysis Plot
The figure below is the 3-panel Stability Analysis output:

- Top: inferred singularity year $t_0$ over inference windows (with HDI bands).
- Middle: inferred growth exponent $\alpha$ over inference windows (with HDI bands).
- Bottom: training MAPE with gating threshold.

![Stability Analysis (3-panel)](stability_analysis.png)

## Repository Contents
- `analysis.ipynb`: full end-to-end notebook implementation.
- `singularity_analysis_spec.md`: original project specification and milestone requirements.
- `stability_analysis.png`: exported Stability Analysis plot used in this README.

## Notes
Recent windows in the rolling analysis show deterioration in training fit (higher MAPE), which is consistent with the hypothesis that global dynamics have shifted away from a stable hyperbolic regime.