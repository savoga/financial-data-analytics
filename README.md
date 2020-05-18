# Financial data analytics

I've initially done this project for an interview. I then wanted to review it and push the analysis further.

## Context

I used <a href="https://spark.apache.org/">spark framework</a> in order to easily perform computation on datasets. Altough better adapted for larger datasets, the use of the framework was a requirement for this exercise.

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

### Multiclass approach

### Binary approach

### Model selection

### Conclusion
