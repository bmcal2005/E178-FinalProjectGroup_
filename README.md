# E178 Final Project: Predicting Wildfire Ignition from Weather

**Question:** Does daily weather predict whether a wildfire starts in California better than the calendar alone?

## Data
`CA_Weather_Fire_Dataset_1984-2025.csv`: daily weather (precipitation, max/min temperature, wind, 7-day lagged features) with a `FIRE_START_DAY` label, 1984-01-01 to 2025-01-12.

Data health findings (see notebook, Section 2):
- 13 rows have missing weather values. They are dropped.
- The fire label is not populated after Dec 2023 (2024–2025 are all `False`), so those years are excluded.
- The fire-day rate trends upward over time, so we use a time-based split: train 1984–2015, test 2016–2023.

//## Performance metric
- **Primary:** PR-AUC (average precision) on 2016–2023, compared against a month-only baseline.
- **Secondary:** ROC-AUC, plus recall, precision and a confusion matrix at a chosen threshold.

| Model | PR-AUC | ROC-AUC |
|---|---|---|
| No-skill (base rate) | 0.460 | 0.500 |
| Month only (logistic) | 0.782 | 0.832 |
| Weather only (logistic) | 0.771 | 0.821 |
| Weather + month (logistic) | 0.810 | 0.849 |
| Random forest, tuned with TimeSeriesSplit (weather + month) | 0.805 | 0.848 |

At a threshold chosen on 2011–2015 validation for 80% recall, the random forest gets **82.7% recall and 70.5% precision** on the 2016–2023 test set (F2 = 0.80).//

## Running
```bash
pip install numpy pandas matplotlib scipy scikit-learn jupyter
jupyter notebook wildfire_ignition_E178.ipynb
```
Keep the CSV in the same folder as the notebook.
