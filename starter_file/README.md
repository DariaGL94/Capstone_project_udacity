*NOTE:* This file is a template that you can use to create the README for your project. The *TODO* comments below will highlight the information you should be sure to include.

# Heart Failure prediction - Capstone project

*TODO:* Write a short introduction to your project.
In this project I do a simple custom logistic regression to predict heart failure based on 12 features and then optimize hyperparameters by using HyperDrive. In addition, I train classification models using AutoML and compare the results. The model with the best accuracy will be deployed.

## Project Set Up and Installation
*OPTIONAL:* If your project has any special installation steps, this is where you should put it. To turn this project into a professional portfolio project, you are encouraged to explain how to set up this project in AzureML.

## Dataset

### Overview
*TODO*: Explain about the data you are using and where you got it from.
I'm using the heart failure prediction dataset from kaggle (https://www.kaggle.com/datasets/andrewmvd/heart-failure-clinical-data?resource=download). People with a high cardiovascular risk due to the presence of risk factors (detected by this ML model) can prevent a heart failure by changing some behavior.

### Task
*TODO*: Explain the task you are going to be solving with this dataset and the features you will be using for it.
 This dataset contains 12 features such as age, diabetes, high blood pressure etc. It's a classification task to predict mortality by heart failure based on these features. I will be using all 12 feautures.
### Access
*TODO*: Explain how you are accessing the data in your workspace.
I am accessing the data in two different ways. For the automl part, I registered a Tabular Dataset from local files in the workspace. For the HyperDrive part, I uploaded the csv file in starter_files and loaded the dataset as pandas dataframe in the train_lr.py script.

## Automated ML
*TODO*: Give an overview of the `automl` settings and configuration you used for this experiment
I chose 'accurancy' as primary metric because it is a good meassure of a classification task. In addition, it is easier to compare the both models later without having a look at all metrics (the model with the bist accuracy should be deployed). To ensure that the training of the model does not take too long (due to costs), I decided that the experiment should time out after 20 minutes and maximum 5 iterations to test in the training job. this number should be not greater than the number of nodes in the compute cluster.

Here, it is a classification task to predict the column 'DEATH_EVENT' in the dataset. The early stopping is enabled to save unnecessary running time and to ensure that the best accuracy is reached. The automatic featurization is enabled, so data guardrails and feauturization steps are done automatically in the preprocessing.

### Results
*TODO*: What are the results you got with your automated ML model? What were the parameters of the model? How could you have improved it?

The best model from AutoML run has an accuracy of 0.88644. For me, the most suprising result is the feature importance. Hence, diabetes and smoking do not have a large impact on heart failure - as I thought it would be. There are a lot of interesting insights on the model's performance (screenshots best_model_automl - best_model_automl7). The model is a VotingEnsemble consisting of different algorithms with different ensemble weights. The models performance can be improved by paying more attention to the data - like looking for outliers or imbalances.

*TODO* Remeber to provide screenshots of the `RunDetails` widget as well as a screenshot of the best model trained with it's parameters. -> RunDetails_automl

## Hyperparameter Tuning
*TODO*: What kind of model did you choose for this experiment and why? Give an overview of the types of parameters and their ranges used for the hyperparameter search
For the classification task I'm using a simple logistic regression. It's a good model to start with. The hyperparameter C is an inverse of regulation strength and max_iter is the maximum numbers of iterations to converge. Smaller values of C cause stronger regularization. The Bandit policy prevents that an algorithm is taking too many iterations to every posiible combination of hyperparameters without producing better results. It is based on factor/slack amount of the most successful job. The Random sampling supports discrete (max_iter) as well as continuous (C) hyperparameters. Random sampling can be used as an initial search with an later refinement for better results. It also supports early termination, so it is a low-budget solution in contrast to Grid sampling or Bayesian sampling. In addition, I chose accuracy as primary metric because it is a good meassure for a classification and 10 as a total number of run, so the experiment does not take too long.


### Results
*TODO*: What are the results you got with your model? What were the parameters of the model? How could you have improved it?
The best model has the hyperparameters C=2.5568773701545626 and max_iter=120. The accuracy is 0.8. So the model from the automl run performs better. I could change the hyperparameter spaces to improve the model. In this case, I think that a logistic regression might not be the right choice for this data. Maybe trying other models could lead to better results.

*TODO* Remeber to provide screenshots of the `RunDetails` widget as well as a screenshot of the best model trained with it's parameters. -> RunDetails_hyperdrive

## Model Deployment
*TODO*: Give an overview of the deployed model and instructions on how to query the endpoint with a sample input.
I deployed the best model from automl Run as a Webservice with enabled authentication. To query an endpoint with sample input, we need first to get a scoring uri and the primary key from the endpoint workspace section. Then, we need to define some sample data in the right form (here swagger can help us). This data is converted to a JSON string and a request ist made. 

## Screen Recording
*TODO* Provide a link to a screen recording of the project in action. Remember that the screencast should demonstrate: https://youtu.be/2lS0v1ZM3Tg
- A working model
- Demo of the deployed model
- Demo of a sample request sent to the endpoint and its response

## Standout Suggestions
*TODO (Optional):* This is where you can provide information about any standout suggestions that you have attempted.
