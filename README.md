# Predicting Used Car Prices 🏎️ 🚗 🚙
The model is currently deployed on https://estimateur-prix-voiture.herokuapp.com/ *(in french)*, and the building repository is available [here](https://github.com/pcmaldonado/Estimateur_prix_voiture).

## Data
The data was collected from a popular french ad site (almost 50,000 ads were collected) and ten different features were extracted to later predict prices (brand, kms, number of doors, etc.). 

Complementary data about car brands was scraped from different sources (origin & type of brand).

### Use of:
* Python Version: 3.9.7
* Packages: numpy, pandas, matplotlib, seaborn, sklearn, pickle, joblib, feature_engine, scikit_learn, xgboost, shap

# Overview
* The objective of this analysis is to predict and better understand the variables that determine the selling price of used cars. 
* To do so, feature engineering & feature selection was conducted to clean the dataset
* New features were added to better estimate the relationship between brands & price
* The best algorithm and its optimal parameters were chosen using k-fold cross validation 
* The final model is a XGBoosting regressor with a MAE score of 3803 and a R² of 0.87 on test data *(for  reference, mean test price: 26,155 and std price: 24,773)*
* The top 3 most important features were whether the car had a manual transmission, if the brand was considered as "luxury" and if the car came from Germany

One way to further improve the performance would be to collect more data, introduce/keep more features and possibly build different regression algorithms trained for different brands to get more detailed information without risk of overfitting.


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