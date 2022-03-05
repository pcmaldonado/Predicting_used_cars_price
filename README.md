# Predicting Used Car Prices 🏎️ 🚗 🚙
The model is currently deployed on https://estimateur-prix-voiture.herokuapp.com/ *(in french)*, and the code used for deployment is available [here](https://github.com/pcmaldonado/Estimateur_prix_voiture).

## Data
The data was collected from a popular french ad site (almost 50,000 ads were collected) and ten different features were extracted to later predict prices (brand, kms, number of doors, etc.). 

Complementary data about car brands was scraped from different sources (origin & type of brand).

### Use of:
* Python Version: 3.9.7
* Packages: numpy, pandas, matplotlib, seaborn, sklearn, pickle, joblib, feature_engine, scikit_learn, xgboost, shap

## Overview
* Created a tool that estimates the selling price of used cars to help people evaluate their cars before selling or buying one
* Scraped around 50,000 ads of selling cars from a popular french website using python (BeautifulSoup)
* Engineered features from additional scraped data on brands (luxury brands & origin)
* Explored 9 different models, then applied GridsearchCV to the better performing ones to get the best model
* Built a site using Flask that lets users input car features to get an estimate of its selling price

<br>

## [EDA & Preprocessing](https://github.com/pcmaldonado/Predicting_used_cars_price/tree/main/EDA_Preprocessing)
The preprocessing was done in two steps:
* First, an Exploratory Data Analysis was conducted on the training set to know how to best clean the data
* Then, preprocessing was applied to the raw training set

The preprocessing consisted on applying feature engineering:
* target value & "Kms" were log-transformed for better spread
* missing values were handled according to type of data (numerical/categorical) and the % of missing data
* new features were created based on data from brands & complementary data
* one hot encoding & frequent encoding were applied when needed
* feature scaling was applied
The second step also included applying a robust scaler to both the training and the test sets.

<br> ==>The final trainining set contains 43,713 rows and 25 features.

    

## [Regression Analysis](https://github.com/pcmaldonado/Predicting_used_cars_price/tree/main/Modelling_RegressionAnalysis)
The regression analysis included 4 steps:
* Choosing the metrics used for performance: RMSE, MAE & R²
* A first round of modelling as benchmark using 9 different algorithms (LinearRegression, KNeighborsRegressor, AdaBoost, ...)
* The 2 best models were fine tuned using k-fold cross validation (ExtraTrees & XGBoosting)
* After comparison, the best model was saved and later applied to the test set

Following the steps of the preprocessing, a pipeline was constructed to clean the test set and apply the fine-tuned model.

The final performance (measured after applying the final model on the test set) is:
| Models           |  RMSE score | MAE score   |  R² score  |  
| :--------------: | :---------: | :--------:  | :--------: | 
| XGBRegressor     |  8883       |  3803       |  0.871     | 