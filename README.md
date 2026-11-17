# Hospital Consultation Forecasting using XGBoost

A Google Colab project demonstrating monthly hospital consultation forecasting using **XGBoost**. The workflow consolidates 11 original channel categories into six main channels, performs walk-forward validation, compares XGBoost with a Seasonal Naive baseline, and exports predictions and evaluation metrics to Excel.

## Main features

- Reads an Excel workbook (`Sheet1`) with `Date`, `companychannel`, and `Nos` columns (the notebook also expects the supporting date columns to be present in the sample file).
- Maps original channel categories into six reporting channels:
  - **Cash:** `CASH`, `Cash`, `Clinic`, `Embassy`, `Research`
  - **CGHS:** `ECHS`, `NHA-CGHS`, `NHA-PSU`, `PSU`
  - **Corporate:** `Corporate`
  - **EWS:** `EWS`
  - **International:** `International`
  - **TPA:** `TPA`
- Aggregates daily consultations and sets Sundays to zero.
- Builds monthly lag, rolling-average, trend, and seasonal features.
- Trains an XGBoost regressor separately for each main channel.
- Performs January–October 2026 walk-forward validation.
- Compares XGBoost with a Seasonal Naive benchmark.
- Calculates MAE, RMSE, WAPE, MAPE, and R².
- Displays three graphs: overall monthly comparison, channel-wise comparison, and monthly XGBoost WAPE.
- Provides a month dropdown for predictions and an Excel export button.

## Repository structure

```text
Hospital-Consultation-Forecasting-XGBoost/
├── Consultation_Forecasting_XGBoost.ipynb
├── README.md
├── requirements.txt
├── data/
│   └── synthetic_consultation_data.xlsx
└── outputs/
    └── .gitkeep
```

## Run in Google Colab

1. Upload or open `Consultation_Forecasting_XGBoost.ipynb` in Google Colab.
2. Run the notebook from top to bottom.
3. When prompted, upload `data/synthetic_consultation_data.xlsx` to test the workflow.
4. Review the validation metrics and graphs.
5. Select a target month from the dropdown and click **Predict Month**.
6. Predict any other required months, then click **Give Excel**.

The notebook's current interactive month selector is configured for 2026. The example dataset spans 2023–2026 so there is enough history for the model.

## Evaluation metrics

- **MAE:** Mean Absolute Error, in consultation counts.
- **RMSE:** Root Mean Squared Error, in consultation counts; penalizes larger errors more.
- **WAPE:** Sum of absolute errors divided by total actual consultations.
- **MAPE:** Mean absolute percentage error for observations with non-zero actual values.
- **R²:** Coefficient of determination; can be negative.

The benchmark uses the corresponding month's actual value from the previous year. Compare metrics on the same validation months.

## Data privacy and synthetic sample

`data/synthetic_consultation_data.xlsx` is newly generated demonstration data with the same 11 observed original category names and compatible column structure. It is **not** a copy of the source hospital workbook and does not contain patient-level records.

Do not commit the original hospital workbook, real hospital consultation reports, patient identifiers, or confidential business data to a public repository. Keep real-data validation figures private unless you have explicit authorization to publish them.

## Dependencies

Install the libraries listed in `requirements.txt`. The notebook also installs XGBoost and uses Google Colab's upload/download utilities.

## Limitations

- The model is trained independently for each of the six consolidated channels.
- The validation window shown in this notebook is January–October 2026.
- The November partial-month adjustment is a weekday-weighted estimate used for December training.
- Validation metrics describe the selected historical validation period; they are not a guarantee of future performance.
