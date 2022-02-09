# Preprocessing -overview
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

<br><br>
# More information

## Dropping unnecessary features
* **Id**, **Url**, **Region_url**, **Image_url** and **VIN** were dropped because they did not provide with any useful information
* **Description** was dropped because, although some descriptions seem like spam, without a target value, it would be very subjective to measure any kind of filtering; moreover, descriptions contains data encoded on other features of the dataset
* **Posting_date** was dropped as any kind of seasonal analysis would be meaningless as all records came from the same month
* **Size** and **county** were dropped because they contained high % of null values (70%, and 100% resp.)
* **Long** and **Lat** were dropped because they did not seem to contain robust data  (coordinates do not match "state" or "region" data)
* **Region** was dropped as it contained more than 400 cities --**state** was kept to provide locations
* **Model** was dropped as it was not standardized (same car models were present with different names) and contained too much detailed data (almost 24000 models were present, with some models counting thousands of cars, and others just 1).

## Filtering "Price" & "Odometer"
Although it can be a good thing to keep outliers, as they reflect real world data, some values present in the dataset could not reflect real data and were just noise (ex: max price was $3.7 billions --knowing that most expensive car ever sold was sold at an auction at $48.4 millions).

To avoid having too much noise, 
* "Price" was filtered to only contain prices above $1,000 and below $70,000 (39,672 rows dropped)
* "Odometer" was filtered to contain only values below 1,000,000 (300 rows dropped)

## Encoding categorical features
* **Cylinders** contained str values showing the number of cylinders a car had (ex: "*6 cylinders*"). The digits were extracted from the strings in order to obtain only the numerical values ("*6 cylinders*" => *6*).
* **Condition** contained ordinal data (categories with implied order: *"salvage"*, *"fair"*, *"good"*, *"excellent"*, *"like new"*, *"new"*) which was converted to numerical values (from 0 to 5).
* **Fuel** contained 5 categories of nominal data.Since, "*gas*" was very predominant (253,411), and "other" had the second place (21,619), the feature was converted into a "Dummy variable" with "1" for "*gas*" values and "0" for all the other categories.
* **Title_status** was also modified to be "One-Hot encoded". Values "Clean" and "Rebuilt" mean Title_status_ok = 1, and values "*salvage*", "*lien*", "*missing*", "*parts_only*" mean 0.
* **Transmission** was also modified into a dummy variable, following the same pattern than "Fuel", as one category was clearly more frequent than the others ("*automatic*": 233,611; "*other*":48,336, "*manual*":18,153)
* **Type** contained 13 different values. The top 4 values were kept and the remaining were encoded as "*other*"
* **Paint_color** contained 12 different values. First "*silver*" and "*grey*" were treated as one ("grey"), then only the top 4 colours were kept --the rest became "*other*".

## Synthesizing features using external/scraped data
* **States** data was converted to general regions of the US (**US_region**: South, Midwest, West, Northeast) using data available [here](https://github.com/cphalpert/census-regions).
* **Manufacturer** data, which contained 42 brands,  was converted into brand origin continent ("*Chevrolet*" => "*North America*") using scraped data from [Wikipedia](https://en.wikipedia.org/wiki/List_of_current_automobile_manufacturers_by_country)

## Dealing with missing data
* First, rows containing too much missing data ("nan" in more than 4 features --from 13) were dropped.
* Second, One-Hot Encoding was applied to have only numerical values.
* Third, the IterativeImputer from sklearn was used to impute missing values.