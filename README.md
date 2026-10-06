# Upper-Funnel-Modeling

Repository showing how to give upper funnel channels their true credit.

## What's in here

[analysis.ipynb](analysis.ipynb) builds two Marketing Mix Models (MMMs) on the same four-year weekly dataset and compares them:

1. **Standard model** — `search_spend` is treated as an ordinary channel, like every other.
2. **Funnel-aware model** — `search_spend` and `search_impressions` are modeled as a mediator: upper-funnel spend (`Broadcast`, `Streaming`) feeds a hidden search-demand signal, which reaches `revenue` both directly and indirectly through search.

Two lower-funnel channels (`DirectMail`, `Digital`) only ever have a direct effect on revenue in either model.

The notebook walks through: the causal story and a diagram of it, loading the data, exploratory analysis, a custom `pymc-marketing` `MuEffect` that implements the mediator, fitting both models, sampler diagnostics, posterior predictive checks, splitting the funnel-aware model's upper-funnel credit into direct vs. indirect parts, a head-to-head comparison (contribution + ROAS) between the two models, and in-sample/holdout predictive fit (RMSE, R², MAPE, CRPS).

This notebook is scoped to **attribution only** — no ROAS calibration from experiments, no cross-validation, no budget optimization.

## Data

[data/media_data.csv](data/media_data.csv) is weekly data (four media channels, two search columns, grouped holiday flags, and revenue) derived and anonymized from a public case-study dataset: values are rescaled/jittered, channels and holidays are grouped under new names, and dates are shifted into recent years. See [scripts/anonymize_data.py](scripts/anonymize_data.py) for how it was produced (not tracked in git — see `.gitignore`).

## Setup

Clone the repo and install the dependencies:

```bash
git clone <this-repo-url>
cd Upper-Funnel-Modeling
pip install -r requirements.txt
```

`graphviz` (the Python package) also needs the system `dot` binary to render the diagram in the notebook (e.g. `brew install graphviz` on macOS, `apt install graphviz` on Debian/Ubuntu).

Then open [analysis.ipynb](analysis.ipynb) and run top to bottom; fitting both models (and their holdout refits) can take a while, so lower `draws`/`tune` in the sampling cells for a quicker pass.
