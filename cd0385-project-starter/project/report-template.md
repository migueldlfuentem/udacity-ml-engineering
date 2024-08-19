# Report: Predict Bike Sharing Demand with AutoGluon Solution
#### NAME Miguel de la Fuente

## Initial Training
### What did you realize when you tried to submit your predictions? What changes were needed to the output of the predictor to submit your results?
I had to set negative values to zero as Kaggle doesn't accept negative values.

### What was the top ranked model that performed?
It was "WeightedEnsemble_L3"

## Exploratory data analysis and feature creation
### What did the exploratory analysis find and how did you add additional features?
I found that two columns could be categorical and I also split the date column to add additional features, such as day, month or hour.

Playing with the different features created I found that the better performance I achieve was using hour as a integer column, as I also trained the model converting this column to categorical but I get worse performance.

### How much better did your model preform after adding additional features and why do you think that is?
I trained the model utilizing the three previously mentioned features and experimented with different data types. Ultimately, the best performance was achieved by using the "hour" feature as an integer column. The inclusion of this feature led to more than 50% improvement in model performance. This enhancement likely stems from the model's improved ability to discern patterns associated with demand fluctuations at different hours of the day.

## Hyper parameter tuning
### How much better did your model preform after trying different hyper parameters?
It improves a lot. It gets to 0.60 from 1.83. I tried different hyperparameter configuration for each model and they are detailed below:

- GBM: I include "extra_trees" and "GBMLarge" in order to add randomness to the splits, reducing overfitting ("extra_trees") and to capture more complex patterns by using a larger model with more capacity.
- CAT: Catboost is strong in handling categorical data, such as season and weather. I reduce "learning_rate" to avoid overfitting and apply "l2_leaf_reg" to adds regularization.
- XGB: I set a bunch of parameters, highlighting among them 'max_depth' to reduce complexity, 'subsample' and 'colapse_bytree' to use all the data and features for each tree and 'gamma' to add regularization to control overfitting.
- FASTAI: I include moderate learning rate an enough epochs to allow the model to learn without overfitting.
- RF & XT: I increase the number of estimators to ensure that the models are stable and not overly sensitive to random variations in the data.
- KNN: I keep the original configuration as every configuration I used affected the model accuracy negatively.

The configuration that improve the prediction the most was the added to FastAI as it improves the accuracy in more than 50%.

I also discover that by adding the following configuration to the hyperparameters_tune_kwargs I achieve worse kaggle results as the model is overfitting so it works great for the training data but horrible with the test data.

hyperparameter_tune_kwargs = {
    "num_trials": 5,
    "scheduler": "local",
    "searcher": "auto",
}

Eventually I set the hyperparameter_tune_kwargs to "auto" as it helped me to avoid overfitting, achieving a kaggle score of 0.63741. Anyway the best result I obtained was using the hyperparameters without hyperparameter_tune_kwargs.

### If you were given more time with this dataset, where do you think you would spend more time?
I think I would have spent more time playing with the hyperparameters so I could the performance of the model

### Create a table with the models you ran, the hyperparameters modified, and the kaggle score.
|model|hpo1|hpo2|hpo3|score|
|--|--|--|--|--|
|initial|n_neighbors=5|n_estimators=100|learning_rate=0.3|1.79674|
|add_features|n_neighbors=5|n_estimators=100|learning_rate=0.3|0.65967|
|hpo|n_neighbors=10|n_estimators=300|learning_rate=0.1|0.60567|


### Create a line plot showing the top model score for the three (or more) training runs during the project.

![model_train_score.png](img/model_train_score.png)

### Create a line plot showing the top kaggle score for the three (or more) prediction submissions during the project.

![model_test_score.png](img/model_test_score.png)

## Summary
During this exercise I learned how to use different configurations of hyperparameters and how to properly apply EDA in order to improve models performance and avoid overfitting.
