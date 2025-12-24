# Water Quality Classification - Master Project

## Overview
This project develops a machine learning model to classify water quality based on the **GFQA v2 dataset** (Published November 27, 2024). The model predicts binary water quality outcomes (good/fair) using threshold-based feature engineering and classification algorithms.

**Dataset Source:** [GFQA v2 on Zenodo](https://zenodo.org/records/14230628)

## Project Structure

```
master project/
├── data_clean.ipynb          # Dataset cleaning and feature engineering
├── data_vis.ipynb            # Data visualization and analysis
├── train.ipynb               # Model training and evaluation
├── final_dataset.csv         # Processed dataset (all parameter measurements)
├── pivot_dataset.csv         # Pivoted dataset (one row per sample) [https://drive.google.com/file/d/15VKp3W2AhLYFnb9CMCZSGoj1l5L8EAjC/view?usp=sharing]
└── dataset/GFQA_v2/          # Raw GFQA v2 data files (download from https://zenodo.org/records/14230628)
    ├── pH.csv
    ├── Temperature.csv
    ├── Electrical_Conductance.csv
    ├── Dissolved_Gas.csv
    ├── Oxidized_Nitrogen.csv
    ├── Phosphorus.csv
    ├── Optical.csv
    ├── Salinity.csv
    └── [metadata files]
```

## Water Quality Parameters

The project analyzes **8 key water quality parameters**:

| Parameter | Good Range | Category | Purpose |
|-----------|-----------|----------|---------|
| **pH** | 6.5–8.5 | Acid-base balance | Affects toxicity and corrosion |
| **Temperature** | 0–30 °C | Thermal conditions | Impacts dissolved oxygen and metabolism |
| **Electrical Conductance (EC)** | 0–2500 µS/cm | Mineralization | Reflects total dissolved salts |
| **Dissolved Gas** | 5–15 mg/L | Oxygenation | Essential for aquatic life |
| **Oxidized Nitrogen** | 0–10 mg/L | Nutrient levels | Indicates agricultural runoff |
| **Phosphorus** | 0–0.1 mg/L | Nutrient levels | Primary eutrophication driver |
| **Optical (Turbidity)** | 0–5 NTU | Water clarity | Indicates suspended particles |
| **Salinity** | 0–0.5 g/L | Salt content | Affects usability and biodiversity |

## Workflow

### 1. **Data Cleaning** (`data_clean.ipynb`)
- Load raw GFQA v2 CSV files
- Parse and convert numeric values (handle European decimal format)
- Apply rule-based quality classifications:
  - **Good**: Parameter within optimal range
  - **Estimated**: Slight deviation with limited impact
  - **Suspect**: Increased risk conditions
  - **Contamination**: Unsafe levels
- Aggregate measurements per sample using **worst-case logic**
- Create binary target variable: `good` (all parameters Good) vs `fair` (any parameter ≠ Good)
- Output: `final_dataset.csv` and `pivot_dataset.csv`

### 2. **Data Visualization** (`data_vis.ipynb`)
- Distribution of quality classes per parameter
- Temporal evolution of water quality
- Final quality distribution (worst-case aggregation)
- Binary target balance analysis
- Correlation heatmap between parameters

### 3. **Model Training** (`train.ipynb`)
- **Random Forest Classifier** with hyperparameter tuning
- Train-test split (80/20) with stratification
- Class balancing to handle imbalanced datasets
- Metrics: Balanced Accuracy, Confusion Matrix, Classification Report
- Feature importance analysis
- Grid Search optimization for best hyperparameters

## Quality Classification Rules

Water quality thresholds are based on international standards and environmental guidelines:

- **Good**: Optimal conditions for aquatic ecosystems and human consumption
- **Estimated**: Slight deviations with manageable environmental impact
- **Suspect**: Conditions indicating potential ecological or health risks
- **Contamination**: Unsafe levels requiring remediation

### Worst-Case Aggregation
When multiple parameters are measured for a single sample, the **worst** quality class is assigned:
- Contamination > Suspect > Estimated > Good

This conservative approach ensures comprehensive risk assessment.

## Binary Target Mapping
Final predictions use a binary classification:
- **good** = All parameters classified as "Good"
- **fair** = Any parameter classified as Estimated, Suspect, or Contamination

## Dependencies

```
pandas
numpy
scikit-learn
matplotlib
seaborn
plyer (for notifications)
```

## Usage

### Run Data Cleaning
Open `data_clean.ipynb` and execute cells sequentially to:
- Load raw GFQA v2 data
- Apply quality thresholds
- Generate processed datasets

### Visualize Data
Open `data_vis.ipynb` to explore:
- Quality distributions
- Temporal trends
- Correlations between parameters

### Train Model
Open `train.ipynb` to:
- Train Random Forest classifier
- Evaluate performance metrics
- Perform hyperparameter tuning
- Analyze feature importance

## Key Findings

- **Binary target distribution**: Analyzed in `data_vis.ipynb`
- **Feature importance**: Random Forest ranks parameters by predictive power
- **Model performance**: Balanced accuracy and classification metrics in `train.ipynb`

## References

- **Dataset**: GFQA v2, Published November 27, 2024
  - [Zenodo Record](https://zenodo.org/records/14230628)
  
- **Water Quality Index Methods**:
  - [WQI Review Paper](https://pmc.ncbi.nlm.nih.gov/articles/PMC10006569)
  - [Groundwater Quality Classification](https://www.mdpi.com/2073-4441/16/12/1666)

## Notes

- The model predicts water quality at a sample level (aggregated across all 8 parameters)
- Threshold-based feature engineering ensures interpretability before ML
- Binary classification maintains consistency with GFQA_v2 labeling scheme
- Detailed quality classes (Good/Estimated/Suspect/Contamination) are preserved for traceability

---

**Last Updated:** December 2025