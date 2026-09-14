# Road Safety Crash Severity Prediction

This project analyzes road-crash data and uses machine learning to classify crash severity as **Minor injury**, **Major injury**, or **Fatal crash**. It was developed as a DataQuest hackathon project and is intended for exploratory analysis and model comparison.

## Project Contents

| File | Description |
| --- | --- |
| [Data Sheet - Sheet1.csv](Data%20Sheet%20-%20Sheet1.csv) | Source dataset with 300 crash records and 14 columns |
| [DataQuest_Megalith.ipynb](DataQuest_Megalith.ipynb) | Complete EDA, feature engineering, hyperparameter tuning, and model evaluation notebook |
| [Resource .pptx](Resource%20.pptx) | Project presentation |

## Dataset

The dataset contains 300 records with 14 original fields and no missing values. The three target classes are balanced with 100 records each.

Original fields include:

- `Crash_Severity` - prediction target
- `Vehicle_Speed`, `Speed_Limit`, `Crash_Time`, and `Age`
- `Gender`, `Vehicle_Type`, `Road_Type`, `Crash_Type`, and `Road_Surface_Condition`
- `Number_of_Lanes` and `Lane_Width`
- `Alcohol_Consumption` and `Seatbelt_Usage`

The notebook derives these additional features:

- `Vehicle_Speed_Range`
- `Age_Range`
- `Lane_Width_Range`
- `Over_Speeding` (`Vehicle_Speed - Speed_Limit`)
- `Over_Speeding_binary`

## Analysis Workflow

The notebook performs the following steps:

1. Loads and inspects the CSV dataset.
2. Checks shape, data types, unique values, descriptive statistics, and missing values.
3. Explores crash severity, speed, time, age, lane width, vehicle type, road conditions, alcohol consumption, and lane configuration through charts and heatmaps.
4. Creates binned and overspeeding features.
5. Label-encodes categorical variables.
6. Splits the data into training and test sets using an 80/20 stratified split with `random_state=42`.
7. Uses Optuna to tune four classifiers.
8. Selects five features for the reduced models: `Vehicle_Speed`, `Crash_Time`, `Age`, `Over_Speeding`, and `Lane_Width`.
9. Compares accuracy, classification reports, and confusion matrices.

## Recorded Model Results

The following results are saved in the notebook for the 60-record test set. They are single-split results, not a cross-validation estimate.

| Model | Accuracy | Macro F1 |
| --- | ---: | ---: |
| Random Forest, reduced features | **0.57** | 0.56 |
| CatBoost, reduced features | 0.55 | 0.55 |
| Random Forest, all features | 0.52 | 0.51 |
| XGBoost, reduced features | 0.48 | 0.48 |
| LightGBM, reduced features | 0.45 | 0.45 |

The recorded results suggest that reducing the feature set improved the Random Forest result from 0.52 to 0.57 on this split. Because the dataset is small, these results should be treated as exploratory rather than production-grade performance claims.

## Setup

Use Python 3.10 or newer when possible. Install the packages used by the notebook:

```powershell
python -m pip install numpy pandas matplotlib seaborn scikit-learn optuna xgboost lightgbm catboost jupyter
```

Then open the notebook in VS Code or Jupyter:

```powershell
jupyter notebook DataQuest_Megalith.ipynb
```

The first notebook cell also installs the four model-tuning libraries. When running outside Google Colab, update the CSV path in the data-loading cell from `/content/Data Sheet - Sheet1.csv` to the location of the CSV in this repository, for example:

```python
df = pd.read_csv("Data Sheet - Sheet1.csv")
```

Run the cells from top to bottom so that the engineered features, encoders, Optuna studies, and trained models are created in order.

## Limitations and Next Steps

- The dataset has only 300 records, so model performance may vary substantially with a different split.
- The notebook uses one train/test split; cross-validation would provide a more reliable estimate.
- No trained model artifact or prediction API is included in the repository.
- Future work could add weather, road geometry, traffic volume, driver fatigue, vehicle telemetry, and richer crash-location data.
- Any real-world deployment would require external validation, monitoring, fairness checks, and appropriate safety review.

## License

No license file is currently included. Add a license before redistributing the project.