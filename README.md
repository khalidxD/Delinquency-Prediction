# Deliquency Prediction Using ML

The project provides a predictive model that can help mortgage lenders and servicers to identify high-risk loans and take appropriate actions to prevent delinquencies and defaults.

The model can also help borrowers to avoid the negative consequences of delinquency, such as credit damage and foreclosure.

In addition, the model can assist regulatory agencies in monitoring and assessing the performance of mortgage markets.

## Understanding the Dataset

The Freddie Mac dataset used in this project is a publicly available dataset provided by Freddie Mac, which is one of the largest mortgage financing companies in the United States.

#### 1) The dataset contains historical loan performance data for single-family residential mortgages that were acquired by Freddie Mac from 1999 to 2020.

#### 2) The dataset includes various features related to each loan, such as

- Loan characteristics (e.g., loan amount, loan type, interest rate, etc.)
- Borrower information (e.g., credit score, income, employment status, etc.)
- Property information (e.g., property type, location, appraised value, etc.).

#### 3) The dataset also includes information on delinquency status, which is defined as the number of months a borrower is behind on their mortgage payments.

## Preprocessing and Sentiment Analysis

#### 1. Data Cleaning: The dataset was checked for missing or null values, and any such values were either imputed or removed based on the percentage of missing values in each feature.

```
train = train.dropna(how='any',axis=0)
```

#### 2. Feature Transformation: The categorical features in the dataset were transformed into numerical features using one-hot encoding. This was done to ensure that the data is suitable for use in machine learning algorithms.

```
from sklearn.preprocessing import LabelEncoder
le = LabelEncoder()
train['isFirstTime'] = le.fit_transform(train['FirstTimeHomebuyer'])
train=train.drop('FirstTimeHomebuyer',axis=1)
```

#### 3. Feature Scaling: The numerical features were then standardized using mean normalization to ensure that all features are on the same scale. This helps to prevent certain features from having a larger impact on the model due to their larger scale.

```
from sklearn.preprocessing import StandardScaler

 scalar=standardScalar()
 X_train=scaler.fit_transform(X_train)
 X_test=scaler.transform(X_test)
```

- **fit_transform()** is used on the training data so that we can scale the training data and also learn the scaling parameters of that data. Here, the model built by us will learn the mean and variance of the features of the training set. These learned parameters are then used to scale our test data.

- **transform()** uses the same mean and variance as it is calculated from our training data to transform our test data. Thus, the parameters learned by our model using the training data will help us to transform our test data. As we do not want to be biased with our model, but we want our test data to be a completely new and a surprise set for our model.

#### 4. Feature Selection: Some of the features in the dataset were not considered important for the prediction of ever delinquency. Therefore, a feature selection method was used to remove the least important features from the dataset. This helps to improve the performance of the machine learning algorithms by reducing the number of features.

<br>
#### 5. Data Splitting: Finally, the dataset was split into training and testing sets. The training set was used to train the machine learning models, while the testing set was used to evaluate their performance. This helps to ensure that the performance of the model is not influenced by the same data used for training.

<br>

```
X_train, X_test, Y_train, Y_test = train_test_split(X, Y, test_size = 0.30, random_state = 0)
```

Overall, these pre-processing steps help to ensure that the data is clean, standardized, and suitable for use in machine learning algorithms. This helps to improve the accuracy and reliability of the models developed using the dataset.

## EDA

**Introduction:**

- Data distribution analysis: The distribution of each feature in the dataset was analyzed to identify any anomalies or outliers. This helps to ensure that the data is suitable for use in machine learning algorithms.

- Correlation analysis: The correlation between each feature in the dataset was analyzed to identify any strong correlations between features. This helps to identify any multicollinearity issues that may affect the performance of the machine learning algorithms.

- Feature importance analysis: The importance of each feature in predicting ever delinquency was analyzed using correlation and heatmaps This helps to identify the most important features that contribute to the prediction of ever delinquency.

- Data visualization: Data visualization techniques such as histograms, scatter plots, and violin plots were used to visualize the distribution and relationship between different features in the dataset. This helps to provide insights into the data and identify any patterns or relationships that may exist.

**Correlation Plot of Numerical Variables:**

The results of the correlation plot analysis showed that some & features were strongly correlated with ever delinquency, while others had a weak or no correlation. For example, the following features were found to have a strong positive correlation with ever delinquency:

- **Original UPB** (Unpaid Principal Balance): This feature indicates the total amount of the loan that remains unpaid. Loans with a high UPB were found to have a higher risk of ever delinquency.

- **Current Interest Rate** : This feature indicates the interest rate on the loan. Loans with a higher interest rate were found to have a higher risk of ever delinquency.

- **Original Loan-to-Value** (LTV) ratio: This feature indicates the ratio of the original loan amount to the appraised value of the property. Loans with a high LTV ratio were found to have a higher risk of ever delinquency.

On the other hand, some features were found to have a negative correlation with ever delinquency. For example, the following features were found to have a strong negative correlation:

- **Loan Age** : This feature indicates the age of the loan in months. Loans with a longer history were found to have a lower risk of ever delinquency.

- **Current Loan-to-Value** (LTV) ratio: This feature indicates the ratio of the current loan amount to the current appraised value of the property. Loans with a low LTV ratio were found to have a lower risk of ever delinquency.

## Model Building

#### Metrics considered for Model Evaluation

**Accuracy , Precision , Recall and F1 Score**

- Accuracy: What proportion of actual positives and negatives is correctly classified?
- Precision: What proportion of predicted positives are truly positive ?
- Recall: What proportion of actual positives is correctly classified ?
- F1 Score : Harmonic mean of Precision and Recall

#### Logistic Regression

- Logistic Regression helps find how probabilities are changed with actions.
- The function is defined as P(y) = 1 / 1+e^-(A+Bx)
- Logistic regression involves finding the **best fit S-curve** where A is the intercept and B is the regression coefficient. The output of logistic regression is a probability score.

#### K-Nearest Neighbor (KNN):

- KNN is a non-parametric classification algorithm that predicts the class of a sample based on the classes of its nearest neighbors in the feature space. KNN can be useful for this project because it does not make any assumptions about the underlying distribution of the data, and it can work well with non-linear decision boundaries. KNN is also relatively easy to understand and can handle large datasets.

#### Naive Bayes:

- Naive Bayes is a probabilistic classification algorithm that calculates the probability of a sample belonging to each class based on its features. Naive Bayes can be useful for this project because it is a simple yet effective algorithm that can work well even with limited training data. It is also relatively fast to train and can handle high-dimensional data.

### Choosing the features

**choose the features** taking in consideration the deployment phase.

The criteria for selecting features for deployment phase is to prioritize relevance, predictive power, stability, feasibility, and interpretability. The goal is to identify a set of features that can be reliably used in a real-world setting to predict loan delinquency with high accuracy and confidence.

When we apply the **logistic regression** model the accuracy is 79.93%.

When we apply **K-Nearest Neighbour** model the accuracy dropped from 79.93% to 76%.

When we apply **Naive Bayes** model the accuracy dropped from 76% to 74%.

So, we will deploy using Logistic Regression model.

```
       precision    recall  f1-score   support

           0       0.84      0.53      0.65     64041
           1       0.24      0.60      0.34     15897

    accuracy                           0.54     79938
   macro avg       0.54      0.56      0.50     79938
weighted avg       0.72      0.54      0.59     79938

```

## Deployment

you can access our app by following this link [stock-price-application-streamlit](https://stock-price-2.herokuapp.com/) or by click [stock-price-application-flask](https://stock-price-flask.herokuapp.com/)

### Flask

- It is a tool that lets you creating applications for your machine learning model by using simple python code.
- We write a python code for our app using Flask ; the app asks the user to enter the following data ( **MIP Units**, **OCLTV**, **DTI**, **OrigUPB**,
  **LTV**, **OrigInterestRate**, **OrigLoanTerm**, **EverDelinquent**, **isFirstTime**, **LTV_Range** ).
- The output of our app will be 0 or 1 ; 0 indicates that the customer/user is Ever Delinquent while 1 means The individual is Not Ever Delinquent.
- The app runs on local host.
- To deploy it on the internt we have to deploy it to Heroku.

### Heroku

We will also deploy our Ever Delinquent prediction app to [ Heroku.com](https://www.heroku.com/). In this way, we can share our app on the internet with others.
We prepared the needed files to deploy our app sucessfully:

- requirements.txt: contains the libraries must be downloaded by Heroku to run app file (stock_price_App_V1.py) successfully
- app.py: contains the python code of a Streamlit web app.
- model_3.pkl : contains our Logistic Regression model that is built by modeling part.
- Team (A) -LR+KNN+NB-2: contains the code of modeling part that can be used to further help improve the modelling part and increase accuracy.
