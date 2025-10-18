# Forest Cover Type Prediction

> Predict the dominant tree species in forest regions using geospatial and environmental features.

![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)
![Pandas](https://img.shields.io/badge/Pandas-150458?style=for-the-badge&logo=pandas&logoColor=white)
![Scikit-learn](https://img.shields.io/badge/Scikit--Learn-F7931E?style=for-the-badge&logo=scikit-learn&logoColor=white)
![Seaborn](https://img.shields.io/badge/Seaborn-000000?style=for-the-badge&logo=seaborn&logoColor=white)

## 📊 Dataset

The dataset contains cartographic measurements from the Roosevelt National Forest (Colorado, USA):
- **Geospatial Features**: Elevation (col 0), Aspect (col 1), Slope (col 2)  
- **Hydrology Distances**: Horizontal (col 3) and Vertical (col 4) distance to nearest surface water  
- **Infrastructure**: Distance to roadways (col 9), fire ignition points (col 5), and wilderness areas (cols 10–13)  
- **Soil Types**: 40 binary indicators (cols 14–53)  
- **Target Variable**: Cover type (col 54) — 7 classes (1=Spruce/Fir, 2=Lodgepole Pine, ..., 7=Krummholz)

> 💡 Dataset is large (581,012 samples), clean (no missing values), but imbalanced—classes 1 & 2 dominate.

## 🧮 Methodology
- **Exploratory Data Analysis**: Visualized correlation matrix of key features  
- **Feature Engineering**: Created 3 new composite features:  
  - `Distance_To_Hydrology` = √(horizontal² + vertical²)  
  - `Fire_To_Road_Ratio` = distance_to_fire / (distance_to_road + 1)  
  - `Elevation_Slope_Ratio` = elevation / (slope + 1)  
- **Preprocessing**: Applied `StandardScaler` to all numerical features  
- **Class Imbalance Handling**: Used **SMOTE** to oversample minority classes in training set  
- **Model Training**: Compared **Random Forest** and **Decision Tree** with `class_weight='balanced'`  
- **Evaluation**: Assessed using **precision, recall, F1-score**, and **confusion matrix** per class

## 📈 Results

### Random Forest Classifier (After SMOTE + Scaling)

| Class | Precision | Recall | F1-Score | Support  |
|-------|-----------|--------|----------|----------|
| 1     | 0.97      | 0.96   | 0.96     | 42,557   |
| 2     | 0.97      | 0.97   | 0.97     | 56,500   |
| 3     | 0.94      | 0.97   | 0.96     | 7,121    |
| 4     | 0.89      | 0.89   | 0.89     | 526      |
| 5     | 0.86      | 0.92   | 0.89     | 1,995    |
| 6     | 0.91      | 0.94   | 0.92     | 3,489    |
| 7     | 0.96      | 0.98   | 0.97     | 4,015    |
| **Accuracy** | — | — | **0.96** | **116,203** |

✅ **Excellent performance across all classes**  
✅ Strong recall for rare classes (4, 5, 6)  
✅ Minimal confusion between ecologically distinct types

### Decision Tree Classifier (After SMOTE + Scaling)

| Class | Precision | Recall | F1-Score | Support  |
|-------|-----------|--------|----------|----------|
| 1     | 0.94      | 0.94   | 0.94     | 42,557   |
| 2     | 0.95      | 0.94   | 0.95     | 56,500   |
| 3     | 0.92      | 0.93   | 0.93     | 7,121    |
| 4     | 0.85      | 0.83   | 0.84     | 526      |
| 5     | 0.79      | 0.88   | 0.83     | 1,995    |
| 6     | 0.86      | 0.89   | 0.87     | 3,489    |
| 7     | 0.93      | 0.96   | 0.94     | 4,015    |
| **Accuracy** | — | — | **0.94** | **116,203** |

⚠️ **2% lower accuracy** than Random Forest  
⚠️ Higher misclassification between similar classes (e.g., class 1 ↔ 2)  
✅ Still robust for real-time use due to fast inference

## ✅ Conclusion

- **Random Forest achieves 96% accuracy** and superior F1-scores—especially for rare cover types (Cottonwood/Willow, Aspen).  
- Engineered features (`Distance_To_Hydrology`, etc.) improved model sensitivity to ecological patterns.  
- **SMOTE + class weighting** effectively mitigated imbalance without overfitting.  
- This system can support automated land mapping, conservation planning, and wildfire risk modeling.  
- **Next steps**: Try XGBoost/LightGBM for further gains, or deploy as a REST API for GIS integration.
