# Regression Analysis
The regression analysis included 5 steps:
* Choosing the metrics used for performance: RMSE, MAE & R²
* A first round of modelling as benchmark using 7 different algorithms (LinearRegression, KNeighbors, RandomForest, AdaBoost, ...)
* Using the model with the highest performance, feature selection was applied and 7 features  were excluded
* The 3 best models of round one were fine tuned using k-fold cross validation 
* A final step of tuning on the best model was done using k-fold cross validation

The final performance (measured after applying the final model on the test set) is:
| Models                    |  RMSE score | MAE score   |  R² score  |  
| :-----------------------: | :---------: | :--------:  | :--------: | 
| RandomForestRegressor     |  3266.72    |  1523.47    |  0.94      | 



**RESULTS:**
* This analysis helps understand the most important features to determine the value of a used car: year (positive effect), odometer (neg. effect), cylinders (pos. effect), condition (pos. effect)
* Applied to a business task, i.e. tool for pricing in a car dealership or vehicle local distribution, better cleaning of the data could be done (better filtering noise, according to the business needs) in order to get more precise estimations which could lead to better performance (more precise).


# Process log
## Getting the Data Ready
The data was cleaned, standardized and splitted into train and test sets. The modelling was done on the training set, hyperparameter tuning was done using k-fold cross validation and final performance was measured on the test set.

## Metrics for performance
To assess performance on a regression task, common metrics are:
   * **Mean Square Error (MSE) / Root MSE (RMSE)**
       * Calculated by the sum of the squares of the prediction errors, thus the lower the value the better the performance. Less robust to outliers, but more computationally efficient. It is generally the prefered metric for regression in the absence of outliers.
   *  **Mean Absolute Error (MAE)** 
       * Obtained by calculating the absolute difference between the model predictions and the true real values, thus as before, lower values mean better performance. Since we take the absolute difference, it will generally give a smaller result than the RMSE. It is more robust to outliers but less computationally efficient.	
   * **Coefficient of determination, R²**
       * R² is a measure of the regression error (difference between predicted value and real value) and the total error (difference between real value and average value). Higher values, up to 1, imply better performance. 
	   
## First Round Modelling
Seven different algorithms were tested on the preprocesed training data using only default parameters.

The results after conducting 5-fold cross validation were:

| Models                    |  RMSE score | MAE score   |  R² score  |  Choice          |
| :-----------------------: | :---------: | :--------:  | :--------: | :--------------: | 
| LinearRegression          |  8523.45    |  6178.59    |  0.60      |  :black_circle:  |
| ElasticNet                |  9893.70    |  7537.52    |  0.46      |  :black_circle:  |
| KNeighborsRegressor       |  4754.03    |  2715.58    |  0.88      |  :green_circle:  |
| DecisionTreeRegressor     |  4556.84    |  1994.70    |  0.89      |  :black_circle:  |
| RandomForestRegressor     |  3349.44    |  1628.56    |  0.94      |  :green_circle:  |
| AdaBoostRegressor         |  8647.48    |  6896.78    |  0.59      |  :black_circle:  |
| GradientBoostingRegressor |  5737.98    |  4000.27    |  0.82      |  :green_circle:  |


## Feature Selection
The Random Forest regressor has the best performance on the training set. For this reason, it will be used to apply feature selection: drop features with low importance in order to simplify the model (lower dimensionality, higher speed, easier interpretations). 

The "Feature importance" property of the Random Forest regressor is an impurity-based metric, also known as Gini importance. The higher the value, the more important the feature is and the sum of "Gini importance" for every feature is 1 (*Note: the importance of continuous variables and categorical variables with high cardinality -many unique values- tend to be overestimated)*. 

<details><summary>Seven features were excluded, with total cumulated importance up to ~0.02:</summary>
<li> <i> paint_color_white </i> </li>
<li> <i> paint_color_black </i> </li>
<li> <i> title_status_ok </i> </li>
<li> <i> paint_color_grey </i> </li>
<li> <i> regions_usa_South </i> </li>
<li> <i> regions_usa_Midwest </i> </li>
<li> <i> paint_color_blue </i> </li>
</details>

To ensure that this exclusion had a small impact on the performance, the models were retrained on the new subset. We can compare the previous result of the best model with the new best results and note that little performance was lost:
| RandomForestRegressor     |  RMSE score | MAE score   |  R² score  |  
| :-----------------------: | :---------: | :--------:  | :--------: | 
| Benchmark results         |  3349.44    |  1628.56    |  0.938      | 
| Feature Selection results |  3516.45    |  1757.28    |  0.932      | 

## Fine tuning hyperparameters
The best models (Random Forest, KNeighbors, Gradient Boosting) were retrained on the remaining training data (i.e. the preprocessed training set minus the excluded features), using GridSearchCV and 3-fold cross validation.

The new results are:
| Models (w/feature selection)     | RMSE score  | MAE score   |  R² score  |  Choice          |
| :-----------------------:        | :---------: | :--------:  | :--------: | :--------------: | 
| RandomForestRegressor w/o tuning |  3516.45    |  1757.28    |  0.932     |  :black_circle:  |
| RandomForestRegressor            |  3390.63    |  1698.16    |  0.937     |  :green_circle:  |
| KNeighborsRegressor              |  3813.63    |  1744.31    |  0.920     |  :black_circle:  |
| GradientBoostingRegressor        |  4667.55    |  3150.94    |  0.880     |  :black_circle:  |

Once more time, the Random Forest model is the clear winner.

## Final round of tuning
Based on previous results, a last round of tunning parameters using 3-fold cross validation and new parameters using GridSearchCV led to the best model for this regression task.

Final results:
| Models                    |  RMSE score | MAE score   |  R² score  |  Choice          |
| :-----------------------: | :---------: | :--------:  | :--------: | :--------------: | 
| RandomForestRegressor     |  3388.41    |  1696.43    |  0.94      |  :green_circle:  |

## Testing real performance
After having trained the model using the training set and k-fold cross validation, the final model was tested on the test set (previously excluded features were also dropped on this set).

The final results on the test set are:
Final results:
| Models                    |  RMSE score | MAE score   |  R² score  |  
| :-----------------------: | :---------: | :--------:  | :--------: | 
| RandomForestRegressor     |  3266.72    |  1523.47    |  0.94      | 
