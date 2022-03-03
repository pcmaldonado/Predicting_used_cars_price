# Regression Analysis
The regression analysis included 4 steps:
* Choosing the metrics used for performance: RMSE, MAE & R²
* A first round of modelling as benchmark using 7 different algorithms (LinearRegression, KNeighbors, AdaBoost, ...)
* The 2 best models were fine tuned using k-fold cross validation 
* After comparison, the best model was saved and later applied to the test set


Following the steps of the preprocessing, a complete pipeline was constructed to clean the test set and apply the fine-tuned model.

The final performance (measured after applying the final model on the test set) is:
| Models           |  RMSE score | MAE score   |  R² score  |  
| :--------------: | :---------: | :--------:  | :--------: | 
| XGBRegressor     |  8883       |  3803       |  0.871     | 
