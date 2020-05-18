# Financial data analytics

I've initially done this project for an interview. I then reviewed it and pushed the analysis further.

## Context

I used <a href="https://spark.apache.org/">spark framework</a> in order to easily perform computation on datasets. Altough better adapted for larger datasets, the use of the framework was a requirement for this exercise. I used a <a href="https://hub.docker.com/r/jupyter/all-spark-notebook/">docker container</a> that contains all the needed libraries (Spark, Jupyter, etc).

Based on a public dataset named <a href="https://sorry.vse.cz/~berka/challenge/pkdd1999/berka.htm">PKDD'99</a>, I first run basic analysis to understand better transactions and loan amounts.

Then I perform a credit risk prediction using several models. Finally, I do a model selection with my model suggestion.

## Data

Data come from the public dataset PKDD'99 and are separated in several files. I work on 2 of them:

- loan.csv
- trans.csv

## Preprocessing

Although my work focus on the 2 above files, I do the preprocessing on all files.

In this part I work on loading the data, casting them in the correct format and removing unusable ones. I choose to remove wrong data format (we can easily find again the wrong records).

For the dates records, I work with String only for a better display.

## Analytics

This section consists of looking at basic statistics such as mean, variance, etc. of the transaction and loan tables.

## Credit risk prediction

The objective is to build a model that classifies if a loan will be paid or not.

### Basic analysis

In this section I conducted simple analysis to understand better the data. 

I focus on the target field that represents the loan status. From the data provider:

- 'A' stands for contract finished, no problems,
- 'B' stands for contract finished, loan not payed,
- 'C' stands for running contract, OK so far,
- 'D' stands for running contract, client in debt

<p align="center"><img src="https://github.com/savoga/financial-data-analytics/blob/master/img/status-repartition.png"></img></p>

Most clients have a running contract that is OK so far.

Among the sample, 7+5=12% of the loans are missing a payment.

### Feature engineering

In order to predict whether a loan will be paid or not in a relevant manner, I choose to focus on the following features that will be included in my model:

- date when the loan was granted
- amount of money
- duration of the loan
- type of card

Status of paying off the loan will be used as the value to predict.

In this section I focus on operation to prepare the data for machine learning algorithms, that is:

- join
- conversion: I convert status (string) to integer
- standardization: used to make sure all variables contribute equally

### Data visualization

Grouping the data by type, I could show 3 dimensions: type of customer, loan amount and loan status.

<p align="center"><img src="https://github.com/savoga/financial-data-analytics/blob/master/img/amount_type.png"></img></p>

--> most of the clients who cannot pay their loans do not have a card

--> a large proportion of contracts that finished without any issue are for lower amounts

--> no junior client are in debt for paying a contract

Although this graph implies that using the type is a relevant feature, the next will draw the opposite conclusion.

Using the PCA, we can reduce the data to 2 dimensions only (explaining 72% of the variance) and plot them:

<p align="center"><img src="https://github.com/savoga/financial-data-analytics/blob/master/img/pca.png"></img></p>

Based on this graph we can see that the data are largely clusterizable.

### Prediction

I perform the prediction firstly using a multiclass approach, that is predicting A, B, C or D as the loan status. I then do a binary prediction, that is if the loan is paid (A, B, C) or not (D).

#### Linear regression

I choose to start with one of the easiest algorithm; it's also the model I know the best so I am able to better extract information of it.

From the OLS results, we can see that the relationship is highly significant globally since p-value associated with Fisher stat is very low. All variables are significant with date and duration being the most important factors. Type is actually not that important (contrary to what we expected in previous part).

Linear regression gives an accuracy score of 60%. Running a cross validation shows that the variance is high.

Using binary classification gives a strongly better score of 96%.

#### Decision tree

The decision tree is a simple algorithm for non linear relations. It gives an accuracy score of 85% in multiclass approach and roughly 60% in binary.

The tree depth is 15, which is already too high to give a good interpretability:

<p align="center"><img src="https://github.com/savoga/financial-data-analytics/blob/master/img/tree.png"></img></p>

#### AdaBoost

AdaBoost is an algorithm belonging to boosting methods; it may be well adapted with such few data as we won't be penalised by the computational time.

It gives 69% in multiclass and roughly 80% in binary after conducting a grid search.

#### KNN

KNN algorithm gives high score in binary and multiclass approach. As seen previously, clustering methods seem a good option for this problem.

### Model selection

<p align="center"><img src="https://github.com/savoga/financial-data-analytics/blob/master/img/ROC.png"></img></p>

### Conclusion

With binary approach, those predictions can lead to 2 types of errors:

- False positive: the model wrongly predicted that the client will pay its loan
- False negative: the model wrongly predicted that the client won't pay its loan

A bank would probably want to make sure the loans are indeed paid. They will thus be in favor of an algorithm that minize the first error (low false positive).

Thus, AdaBoost seems the best algorithm.

Limits: explainability, computational time with more data. 
