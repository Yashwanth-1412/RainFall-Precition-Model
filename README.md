# Rainfall Prediction using Machine Learning

A comprehensive machine learning project that predicts rainfall patterns using historical weather data from Indian cities, with a focus on Hyderabad's meteorological data.

## 🎯 Project Objective

The primary objective of this project is to develop a machine learning model that can accurately predict rainfall based on historical weather data. By using various data preprocessing techniques and exploring different machine learning algorithms, the goal is to create a model that can help in forecasting rainfall patterns. This can be particularly useful for:

- **Water Resource Management**: Reservoir and water supply planning
- **Disaster Preparedness**: Early warning systems for flooding

## 📊 Dataset

**Source**: [Kaggle - Historical Weather Data for Indian Cities](https://www.kaggle.com/datasets/hiteshsoneji/historical-weather-data-for-indian-cities)

**Focus**: Hyderabad weather data converted from hourly to daily basis

The dataset includes various meteorological features such as:
- Temperature (max, min, feels like, heat index)
- Humidity and pressure measurements
- Wind speed and direction
- Cloud cover and visibility
- Precipitation data
- UV index and sun hours

## 🔧 Dependencies

```python
import os
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
from sklearn.preprocessing import StandardScaler, MinMaxScaler, OneHotEncoder, TargetEncoder, FunctionTransformer
from sklearn.compose import ColumnTransformer
from sklearn.impute import SimpleImputer
from sklearn.pipeline import Pipeline, make_pipeline
from sklearn.utils import resample
from sklearn.svm import SVC, SVR
from sklearn.model_selection import cross_val_score, GridSearchCV, RandomizedSearchCV, train_test_split
from sklearn.ensemble import AdaBoostClassifier, RandomForestClassifier, GradientBoostingClassifier
from sklearn.metrics import f1_score, precision_score, accuracy_score, precision_recall_curve
from sklearn.decomposition import PCA
from imblearn.combine import SMOTEENN
```

## 🔄 Data Processing Pipeline

### 1. Data Collection & Formatting
- Downloaded historical weather data from Kaggle API
- Focused on Hyderabad city data
- Converted hourly data to daily aggregations
- Created features for first and second half of the day

### 2. Feature Engineering
- **Temperature Features**: Max, min, feels like, heat index, wind chill
- **Humidity Features**: First half and second half of day averages
- **Wind Features**: Speed and direction for different time periods
- **Cloud Cover Features**: Morning and evening cloud coverage
- **Target Variables**: 
  - `Raintomorrow`: Binary classification (0/1)
  - `RainCategorytomorrow`: Multi-class classification (No Rain, Light Rain, Moderate Rain, Heavy Rain)

### 3. Data Preprocessing
- **Missing Value Handling**: Median imputation strategy
- **Feature Scaling**: 
  - MinMax scaling for bounded features (sunHour, uvIndex, cloudcover, humidity, wind direction)
  - Standard scaling for normal distributions (temperature, pressure, wind speed)
  - Log transformation for skewed features (precipitation, visibility)
- **Class Balancing**: SMOTEENN (SMOTE + Edited Nearest Neighbors) for handling imbalanced classes
- **Dimensionality Reduction**: PCA analysis with 99% variance retention (9 components)

### 4. Feature Selection
Removed highly correlated and low-impact features:
- `moon_illumination`: Very low correlation with target
- `FeelsLikeC` and `HeatIndexC`: Perfect correlation with temperature
- `WindChillC`: Highly correlated with temperature features

## 🤖 Machine Learning Models

### Support Vector Classifier (SVC)
- **Configuration**: RBF kernel with random hyperparameters
- **Performance**: 
  - Accuracy: 82.09%
  - F1 Score: 77.14%
- **With PCA**: Accuracy: 82.21%

### Random Forest Classifier
- **Hyperparameter Tuning**: RandomizedSearchCV with 50 iterations
- **Best Parameters**: 
  - n_estimators: 200
  - max_depth: 30
  - min_samples_split: 2
  - min_samples_leaf: 2
  - max_features: 'log2'
- **Performance**: 
  - F1 Score: 78.39%
  - Accuracy: 83%
- **Class Weighted Version**: Enhanced performance on minority class

## 📈 Model Evaluation

### Performance Metrics
- **Accuracy Score**: Overall correctness of predictions
- **F1 Score**: Harmonic mean of precision and recall
- **Precision-Recall Analysis**: Detailed performance on positive class
- **Cross-Validation**: 5-fold CV for robust performance estimation

### Visualization
- Correlation heatmaps for feature analysis
- Distribution plots for feature engineering validation
- PCA explained variance plots
- Class distribution analysis

## 🚀 Getting Started

### Prerequisites
```bash
pip install pandas numpy scikit-learn matplotlib seaborn imbalanced-learn
```

### Setup Kaggle API
1. Install Kaggle API: `pip install kaggle`
2. Place your `kaggle.json` in `~/.kaggle/`
3. Set permissions: `chmod 600 ~/.kaggle/kaggle.json`

### Running the Project
```python
# Download dataset
dataset_dir = os.path.join(os.getcwd(), "Datasets")
if not os.path.exists(dataset_dir):
    os.makedirs(dataset_dir)

exit_code = os.system(
    f"kaggle datasets download hiteshsoneji/historical-weather-data-for-indian-cities -p {dataset_dir} --unzip"
)
```

## 📁 Project Structure
```
rainfall-prediction/
├── Datasets/
│   └── hyderabad.csv
├── notebooks/
│   └── rainfall_prediction.ipynb
├── src/
│   ├── data_preprocessing.py
│   ├── feature_engineering.py
│   └── model_training.py
└── README.md
```

## 🔮 Future Improvements

- **Advanced Models**: Experiment with XGBoost, LightGBM, and deep learning approaches
- **Feature Engineering**: Weather pattern recognition, seasonal decomposition
- **Real-time Prediction**: Deploy model for live weather data prediction
- **Multi-city Analysis**: Extend to other Indian cities for broader applicability

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## 📝 License

This project is licensed under the MIT License - see the LICENSE file for details.

## 🙏 Acknowledgments

- Kaggle for providing the historical weather dataset
- Scikit-learn community for excellent machine learning tools
- Contributors to the imbalanced-learn library for handling class imbalance
