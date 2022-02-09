# Predicting Used Car Prices 🏎️ 🚗 🚙
## Data
The data came from [Kaggle](https://www.kaggle.com/austinreese/craigslist-carstrucks-data). As put in the site, *"this data ... contains most all relevant information that Craigslist provides on car sales including columns like price, condition, manufacturer,"* and more. 

### Use of:
* Python Version: 3.8.10
* Packages: numpy, pandas, matplotlib, seaborn, sklearn, pickle, joblib, lime

# Overview
* The objective of this analysis is to predict and better understand the variables that determine the selling price of used cars. 
* To do so, feature engineering & feature selection was conducted to clean the dataset
* Feature selection included external data on [US states](https://github.com/cphalpert/census-regions) and scraped data from [Wikipedia](https://en.wikipedia.org/wiki/List_of_current_automobile_manufacturers_by_country) on the country of origin of manufacturers.
* The best algorithm and its optimal parameters were chosen using k-fold cross validation 
* The final model is a RandomForest regressor with a RMSE score of 3266.72 a MAE score of 1523.47 and an R² of 0.94
* One way to further improve the performance would be to better specify the business need to better clean the dataset (for example, sample the raw data based on the type of cars sold by a specific car dealership, so that predictions can be improved and its used car sales prices automated)


## [EDA & Preprocessing](https://github.com/pcmaldonado/Predicting_used_cars_price/tree/main/EDA_Preprocessing)
The original dataset, available [here](https://www.kaggle.com/austinreese/craigslist-carstrucks-data), contained 426,880 rows and 26 columns. 

The preprocessing was done in two steps:
* First, an Exploratory Data Analysis was conducted on the training set to know how to best clean the data
* Then, a preprocessing pipeline was created and applied to the raw training set and the test set 

In the first step, the preprocessing consisted in applying feature engineering:
*  Selecting meaningful features and dropping unnecessary features
*  Filtering numerical values to avoid noise
*  Encoding cateogorical features with one-hot encoding
*  Synthesizing features, based on original features, from external and scraped data to get more meaningful features 
	("states" to "US regions" => '*Alaska (ak)*' : '*West*' ; "brands" to "continent origin brand" => '*nissan*' : '*asia*')
*  Handling missing data with an iterative imputer

The second step also included applying a robust scaler to both the training and the test sets.

<br> ==>The final dataset (train set + test set) contains 377,036 rows and 23 columns.

    

## [Regression Analysis](https://github.com/pcmaldonado/Predicting_used_cars_price/tree/main/Modelling_RegressionAnalysis)
The regression analysis included 5 steps:
* Choosing the metrics used for performance: RMSE, MAE & R²
* A first round of modelling as benchmark using 7 different algorithms (LinearRegression, KNeighbors, RandomForest, AdaBoost, ...)
* Using the model with the highest performance, feature selection was applied and 7 features  were excluded
* The 3 best models of round one were fine tuned using k-fold cross validation (RandomForest, KNeighbors, GradientBoosting)
* A final step of tuning on the best model (RandomForest) was done using k-fold cross validation

The final performance (measured after applying the final model on the test set) is:
| Models                    |  RMSE score | MAE score   |  R² score  |  
| :-----------------------: | :---------: | :--------:  | :--------: | 
| RandomForestRegressor     |  3266.72    |  1523.47    |  0.94      | 

**RESULTS:**
* This analysis helps understand the most important features to determine the value of a used car: year (positive effect), odometer (neg. effect), cylinders (pos. effect), condition (pos. effect)
* Applied to a business task, i.e. tool for pricing in a car dealership or vehicle local distribution, better cleaning of the data could be done (better filtering noise, according to the business needs) in order to get more precise estimations which could lead to better performance (more precise).