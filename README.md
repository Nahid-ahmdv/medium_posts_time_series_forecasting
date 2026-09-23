# Medium Posts Time Series Forecasting

Forecasting the daily number of posts published on [Medium](https://medium.com/) with [Prophet](https://facebook.github.io/prophet/), Meta's decomposable time-series forecasting library. At every step, the analysis checks whether the added modeling complexity actually earns its keep against a simple seasonal-naive benchmark.

📄 **Read the full write-up:** https://medium.com/@nahid.nm57/when-prophet-loses-to-repeat-last-week-which-model-is-actually-teaching-you-something-110e060b9b4e?sharedUserId=nahid.nm57

## What's inside

The notebook (`medium_posts_time_series_forecasting.ipynb`) walks through the full workflow:

1. **Data audit and cleaning**: inspecting ~92k raw Medium post records for implausible timestamps, duplicate URLs, and a partially observed final day, then aggregating cleaned records into a daily post-count series (2012-08-15 to 2017-06-26).
2. **Exploratory analysis**: trend, weekday/weekend seasonality, and variance behavior of the daily series.
3. **Prophet modeling**
   - A baseline Prophet model fit with default settings.
   - A Box–Cox-transformed Prophet model to address variance that grows with the level of the series.
   - A 7-day seasonal-naive benchmark for comparison.
4. **Evaluation**: a single 30-day held-out window, plus a 5-fold rolling-origin (expanding-window) evaluation to check whether the single-window result generalizes.
5. **Forecasting**: refitting the selected specification on the full history and producing a 30-day forecast beyond the end of the data, including an illustrative bias-aware back-transformation of the Box–Cox predictions.

**Key finding:** Box–Cox Prophet clearly outperforms baseline Prophet, but the simple seasonal-naive benchmark remains competitive and sometimes wins outright, depending on the forecast origin. That's a reminder to always validate model complexity against a simple baseline before trusting it.

## Dataset

The raw data is `medium_posts.csv`, a tab-separated export of Medium post records (timestamp, source domain, URL), sourced from the public [mlcourse.ai dataset on Kaggle](https://www.kaggle.com/datasets/kashnitsky/mlcourse).

The dataset is not tracked in this repository. To reproduce the notebook, download `medium_posts.csv` from the link above and place it at:

```
Data/medium_posts.csv
```

## Getting started

### Requirements

- Python 3.9+
- Jupyter (Notebook or Lab)

### Installation

```bash
pip install prophet pandas numpy scipy scikit-learn plotly matplotlib jupyter
```

### Running the notebook

```bash
jupyter notebook medium_posts_time_series_forecasting.ipynb
```

## Repository structure

```
.
├── medium_posts_time_series_forecasting.ipynb   # Main analysis notebook
├── Data/
│   └── medium_posts.csv                          # Raw dataset (not tracked; see Dataset section)
└── README.md
```

## References

- Taylor, S.J. & Letham, B. ["Forecasting at Scale."](https://doi.org/10.1080/00031305.2017.1380080) *The American Statistician*, 72(1), 37–45 (2018).
- [Prophet documentation](https://facebook.github.io/prophet/docs/quick_start.html) and [GitHub repository](https://github.com/facebook/prophet).
- [SciPy documentation for `scipy.stats.boxcox`](https://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.boxcox.html).
- Hyndman, R.J. & Athanasopoulos, G. ["Forecasting: Principles and Practice."](https://otexts.com/fpp3/) A thorough, freely available treatment of time-series forecasting fundamentals, including naive benchmarks and cross-validation for time series.
- [mlcourse.ai public course dataset](https://www.kaggle.com/datasets/kashnitsky/mlcourse), source of `medium_posts.csv`.
