# Traffic Volume Forecaster

Next-hour traffic volume prediction on a real highway sensor, using time, weather and past traffic.

## STAR
- **Situation:** Road traffic changes by hour, day and weather. A forecast for the next hour helps with planning, but a simple "same as yesterday" guess is not very accurate.
- **Task:** Build a model that predicts the next hour traffic volume and beats the "same hour yesterday" baseline on data it has never seen.
- **Action:** Used the UCI Metro Interstate Traffic Volume data (48,204 rows). Removed duplicate timestamps, made the series hourly, filled gaps of up to 24 hours by interpolation and dropped longer gaps (44,629 hourly rows kept). Created hour, day of week, month, weather, holiday, lag 1, lag 24, lag 168 and 24 hour average features. Split by time (first 80% train, last 20% test, test starts 2017-09-25) and compared the baseline, Linear Regression and Random Forest using WAPE.
- **Result:** Random Forest reached **4.58% WAPE** against **17.04%** for the same-hour-yesterday baseline (about 73% lower error) and 10.67% for Linear Regression.

## Results (test set, 8,893 hours)
| Model | WAPE |
|---|---|
| Same hour yesterday (baseline) | 17.04% |
| Linear Regression | 10.67% |
| Random Forest | **4.58%** |

## Key insights
- The last hour traffic (lag_1) is by far the strongest feature (about 0.74 importance), then hour of day (about 0.16) and same hour last week (lag_168, about 0.08). Weather and holiday features add very little.
- Error is smallest at night (about 33 vehicles at 3 AM) and largest late evening and in the rush hours (about 263 at 22:00, 237 at 16:00, 232 at 07:00).
- WAPE is used instead of MAPE because traffic volume is close to zero at some night hours, which breaks MAPE.

## Limitations
- This is a one-step-ahead forecast, it uses the real volume of the previous hour. It is not a multi-hour forecast.
- About 12,000 hourly timestamps were missing in the raw data. Gaps up to 24 hours were interpolated and longer gaps were dropped, so the series is not fully raw.
- One road sensor only, so results may not carry over to other roads.

## How to run
1. Open `traffic_forecaster.ipynb` in Google Colab.
2. Runtime > Run all (data downloads from UCI automatically, about 1 minute).
3. If the download fails the notebook says so and uses synthetic data, so check that the first output says "real data loaded".

## Files
- `traffic_forecaster.ipynb` - executed notebook with outputs
- `requirements.txt`

## Author
Akshat Kesharwani
- Portfolio: https://akshatkesharwani-info.github.io/
- GitHub: https://github.com/akshatkesharwani-info
