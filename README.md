# Water Quality Classification - Master Project

## Overview
This project implements machine learning models to classify and predict water quality based on multiple water parameters. It combines data from two sources: the Kaggle Water Potability dataset and the GFQA_v2 dataset.

## Project Structure

### 1. Data Sources

#### Water Potability (Kaggle)
- **Source**: [Kaggle Water Potability Dataset](https://www.kaggle.com/datasets/adityakadiwal/water-potability)
- **Target Variable**: Potability (1 = Potable, 0 = Not Potable)
- **Features**:
  - pH: Water acidity/basicity (0-14)
  - Hardness: Capacity to precipitate soap (mg/L)
  - Solids: Total dissolved solids (ppm)
  - Chloramines: Amount of chloramines (ppm)
  - Sulfate: Dissolved sulfates (mg/L)
  - Conductivity: Electrical conductivity (μS/cm)
  - Organic_carbon: Organic carbon content (ppm)
  - Trihalomethanes: Trihalomethane concentration (μg/L)
  - Turbidity: Light transmission property (NTU)

#### GFQA_v2 Dataset
- **Source**: [GFQA_v2 on Zenodo](https://zenodo.org/records/14230628) (Published November 27, 2024)
- **Parameters**:
  - pH
  - Temperature (°C)
  - Electrical Conductance (μS/cm)
  - Dissolved Gas (mg/L)
  - Oxidized Nitrogen (mg/L)
  - Phosphorus (mg/L)
  - Optical Properties/Turbidity (NTU)
  - Salinity (g/L)

### 2. Data Processing

#### Quality Classification Rules
Each parameter is classified into four quality levels:

**pH**:
- Good: 6.5–8.5
- Estimated: 6.0–6.5 or 8.5–9.0
- Suspect: 5.5–6.0 or 9.0–9.5
- Contamination: <5.5 or >9.5

**Temperature**:
- Good: 0–30°C
- Estimated: 30–40°C or −2–0°C
- Suspect: 40–50°C or −5–−2°C
- Contamination: Outside ranges

**Electrical Conductivity**:
- Good: 0–2500 μS/cm
- Estimated: 2500–3000 μS/cm
- Suspect: 3000–5000 μS/cm
- Contamination: >5000 μS/cm

**Dissolved Gas**:
- Good: 5–15 mg/L
- Estimated: 3–5 or 15–18 mg/L
- Suspect: 1–3 or 18–20 mg/L
- Contamination: <1 or >20 mg/L

**Oxidized Nitrogen**:
- Good: 0–10 mg/L
- Estimated: 10–20 mg/L
- Suspect: 20–50 mg/L
- Contamination: >50 mg/L

**Phosphorus**:
- Good: 0–0.1 mg/L
- Estimated: 0.1–0.2 mg/L
- Suspect: 0.2–0.5 mg/L
- Contamination: >0.5 mg/L

**Turbidity/Optical**:
- Good: 0–5 NTU
- Estimated: 5–10 NTU
- Suspect: 10–50 NTU
- Contamination: >50 NTU

**Salinity**:
- Good: 0–0.5 g/L
- Estimated: 0.5–1 g/L
- Suspect: 1–3 g/L
- Contamination: >3 g/L

#### Data Cleaning Steps
1. **Handling Missing Values**: Class-wise imputation using median values per potability class
2. **Outlier Detection**: IQR method identifies and clips outliers
3. **Data Transformation**: Quality categories mapped to numeric values (1.0, 0.75, 0.5, 0.0)
4. **Feature Engineering**: Temporal features extracted (Year, Month)

### 3. Models Implemented

#### Kaggle Dataset Models
1. **Random Forest Classifier**
   - n_estimators: 100
   - class_weight: 'balanced'
   
2. **XGBoost**
   - n_estimators: 300
   - max_depth: 5
   - learning_rate: 0.05
   - eval_metric: 'logloss'

3. **Decision Tree Classifier**
   - max_depth: 5
   - criterion: 'gini'

4. **Gradient Boosting Classifier**
   - n_estimators: 300
   - learning_rate: 0.05
   - max_depth: 5

5. **LightGBM**
   - n_estimators: 500
   - max_depth: 6
   - learning_rate: 0.05

6. **CatBoost**
   - iterations: 1000
   - learning_rate: 0.05
   - depth: 6

7. **Support Vector Machine (SVM)**
   - kernel: 'rbf'
   - C: 10
   - class_weight: 'balanced'

#### GFQA_v2 Dataset Models
1. **Random Forest**
   - n_estimators: 200
   - class_weight: 'balanced'
   
2. **Logistic Regression**
   - Pipeline with StandardScaler
   - max_iter: 3000
   - solver: 'lbfgs'

3. **Decision Tree**
   - max_depth: unlimited
   - class_weight: 'balanced'

4. **XGBoost**
   - n_estimators: 200
   - max_depth: 6
   - learning_rate: 0.1

5. **LightGBM**
   - n_estimators: 200
   - learning_rate: 0.1

### 4. Model Evaluation Metrics
- Confusion Matrix
- Classification Report (Precision, Recall, F1-score)
- Balanced Accuracy Score
- Cross-Validation Scores
- Feature Importance Analysis

### 5. Imbalanced Data Handling
- **SMOTE** (Synthetic Minority Oversampling Technique) applied to training data
- Stratified train-test split (80-20) to preserve class distribution
- Balanced class weights in models

## Files
- `water_quality_kaggle.ipynb`: Kaggle dataset analysis and modeling
- `gfqa_v2.ipynb`: GFQA_v2 dataset processing and classification

## Key References
- [Water Potability Prediction with SMOTE and Explainable AI](https://onlinelibrary.wiley.com/doi/epdf/10.1155/2022/9283293)
- [Water Quality Index Methods](https://pmc.ncbi.nlm.nih.gov/articles/PMC10006569)
- [Modified WQI for Groundwater Classification](https://www.mdpi.com/2073-4441/16/12/1666)

## Dependencies
- pandas, numpy
- scikit-learn
- xgboost, lightgbm, catboost
- matplotlib, seaborn
- imbalanced-learn (SMOTE)
- joblib

## Usage Notes
- Models are evaluated using balanced accuracy to account for class imbalance
- Feature importance analysis helps identify key water quality indicators
- GFQA_v2 final target reduced to binary (good/fair) for consistency with original labeling
- Quality thresholds based on water quality standards and environmental regulations

