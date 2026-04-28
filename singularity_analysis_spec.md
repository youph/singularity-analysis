# Project Specification: World Population Singularity Analysis

## 1. Executive Summary
This project is a computational post-mortem of the "Population Singularity" theory (Von Foerster, 1960; Club of Rome, 1970s). The goal is to determine exactly when global demographics shifted away from the hyperbolic power-law growth model. We will use Bayesian inference to track the evolution of the "Doomsday" parameter ($t_0$) as a function of the historical time of inference.

## 2. Technical Stack
- **Runtime:** Jupyter Notebook (`.ipynb`)
- **Data:** `pandas` or `polars` for time-series manipulation.
- **Inference Engine:** `PyMC` (v5+) for Bayesian modeling.
- **Visualization:** `Plotly` (interactive) and `ArviZ` (Bayesian diagnostics).
- **Interactivity:** `ipywidgets` or `Panel` for in-notebook controls.

## 3. Data Strategy
- **Source:** [Our World in Data (OWID) - Population Dataset](https://ourworldindata.org/grapher/population)
- **Format:** The agent should implement a robust downloader that handles the OWID CSV format.
- **Range:** 1800 CE to Present (with capability to include long-tail historical data from 10,000 BCE).
- **Cleaning:** Handle non-uniform sampling by treating the data as a true time-series.

## 4. Mathematical & Bayesian Framework
### 4.1 The Hyperbolic Model
The core model is the singularity power law:
$$N(t) = \frac{C}{(t_0 - t)^\alpha}$$

### 4.2 Priors
- **$t_0$ (Singularity Year):** Normal distribution centered on 2040. **Constraint:** $t_0 > \max(t)$ for any given inference window.
- **$\alpha$ (Growth Exponent):** Half-Normal or Exponential prior (Expected value ~1.0).
- **$\sigma$ (Observation Noise):** Half-StudentT to handle historical reporting errors.

### 4.3 Inference Logic
The system must perform a **Rolling Window Inference**:
- For $t_{inference}$ in range [1960, 2026]:
    - Subset data where $t \le t_{inference}$.
    - Perform MAP estimation or NUTS sampling.
    - Extract Mean and 94% HDI (Highest Density Interval) for $t_0$ and $\alpha$.

## 5. Implementation Milestones

### Milestone 1: Data Ingestion & Visualization
- Download and clean OWID population data.
- Create an interactive Plotly chart with a Toggle for **Linear vs. Log** scales.
- Implement a range slider for $t_{start}$ and $t_{end}$.

### Milestone 2: Bayesian Core
- Define the `PyMC` model class.
- Create a fitting function that takes a data subset and returns parameter estimates.
- Implement a "Goodness of Fit" metric (LOO-CV or WAIC).

### Milestone 3: Stability Analysis (The "Shift" Detection)
- Run the rolling window analysis.
- Plot $t_{inference}$ (x-axis) vs. Inferred $t_0$ (y-axis).
- **Expected Result:** A stable $t_0$ indicates a consistent regime; a diverging $t_0$ indicates the "singularity" has been avoided.

### Milestone 4: Bonus - Alternative Models
- Implement a **Logistic Model** ($N(t) = \frac{K}{1 + e^{-r(t-m)}}$).
- Compare the Logistic vs. Power Law models using `pm.compare`.

## 6. UI & Dashboard Requirements
- **Interactive Fit Overlay:** User selects a "Year of Prediction" (e.g., 1970), and the dashboard draws the projected curve based only on data available up to that year.
- **Parameter Heatmaps:** Show the joint distribution of $\alpha$ and $t_0$.