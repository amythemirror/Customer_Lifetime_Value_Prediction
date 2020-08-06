<img src="https://raw.githubusercontent.com/amythemirror/Springboard-Capstone-Two/master/README%20files/customer%20lifetime%20value.png" width=80%>

# Customer Lifetime Value Prediction
Customer Lifetime Value (LTV) is the total worth to a business of a customer over the whole period of their relationship. It is one of the most important marketing metrics as it
provides crucial guidance for customer acquisition initiatives and even overall marketing strategies.

Our goal for this project is to build a supervised classification machine learning model and a regression machine learning model to predict existing customers’ lifetime values based on their shopping behavior.

## [Data Wrangling](https://github.com/amythemirror/Springboard-Capstone-Two/blob/master/Data%20Wrangling.ipynb)
The dataset was retrieved from the [UCI Machine Learning Repository](https://archive.ics.uci.edu/ml/datasets/Online+Retail+II). It contains all the transactions occurred between 12/01/2009 and 12/09/2011 for a UK-based and registered non-store online retail. The company mainly sells unique all-occasion gifts. Many customers of the company are wholesalers.

Some major data wrangling steps including:
* **Removing duplicates and missing values** - Rows with missing customer ID were removed since we did not have the information required to fill in the missing value.
* **Removing test and postage transactions** - Test order and postage transactions should not be incorporated in the customer lifetime value prediction, and were removed too.
* **Adding the Item Total attribute** - Item Total attribute, representing the total revenue for an item in that specific transaction, was created and added to the dataset.
* **Keeping the outliers for feature engineering** - We kept outliers in the Quantity, Price, and Item Total attributes since they were mostly related to cancellations and returns, and were meaningful for the customer LTV prediction.

## [Exploratory Data Analysis](https://github.com/amythemirror/Springboard-Capstone-Two/blob/master/EDA.ipynb)
The box plots of customer LTVs for the top 10 countries where customers reside, sorted by the mean of each country from left to right in descending order.

<img src="https://github.com/amythemirror/Springboard-Capstone-Two/blob/master/README%20files/clv%20by%20country.png">

### Hypothesis Testing
>**H<sub>0</sub>: UK and Germany have the same average customer LTV**  
>**H<sub>1</sub>: UK and Germany have different average customer LTVs**

We did both bootstrapping and t-test for the hypothesis testing. Neither of their p-values was statistically significant to reject the null hypothesis, using 0.05 as the significance level α.

## [Modeling - Classification Model](https://github.com/amythemirror/Springboard-Capstone-Two/blob/master/Modeling%20-%20Classification%20Model.ipynb)
The feature engineering for classification models can be found on a seperate notebook [here](https://github.com/amythemirror/Springboard-Capstone-Two/blob/master/Feature%20Engineering%20-%20Classification%20Model.ipynb).

We evaluated six classification algorithms. Since the dataset was imbalanced, we compared the models using the balanced accuracy, as well as the macro-averaged precision, recall, and F1 scores.

<img src="https://github.com/amythemirror/Springboard-Capstone-Two/blob/master/README%20files/model%20performance%20table.PNG">

To further evaluate, we then plotted confusion matrices for Gaussian Naive Bayes model and XGBoost model, and seleted Gaussian Naive Bayes model as the final classification model since it was able to correctly predict more mid-LTV (label 1) customers than the XGBoost model.

<img src="https://github.com/amythemirror/Springboard-Capstone-Two/blob/master/README%20files/naive%20bayes%20confusion%20matrix.png">

## [Modeling - Regression Model](https://github.com/amythemirror/Springboard-Capstone-Two/blob/master/Modeling%20-%20Regression%20Model.ipynb)
We used *[lifetimes](https://lifetimes.readthedocs.io/en/latest/index.html)* to build two models. These two models together allowed us to predict customer LTVs from transactional data:
* BG/NBD model to predict the number of repeat purchases for each customer
* Gamma-Gamma submodel to predict the average order value in the future for each customer

Judging from the distribution of the predicted customer LTVs vs that of the actual values, the model underpredicted the occurrences of the lower customer LTVs while following the remaining structure of the data:

<img src="https://github.com/amythemirror/Springboard-Capstone-Two/blob/master/README%20files/histogram%20of%20predicted%20vs%20actual%20customer%20ltvs.png">

The model was able to capture the majority of the top 10 most valued customers, and only mispredicted 2 customers:

<img src="https://github.com/amythemirror/Springboard-Capstone-Two/blob/master/README%20files/top%2010.PNG">
