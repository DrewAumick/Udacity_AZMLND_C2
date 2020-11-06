# Operationalizing Machine Learning Project

In this project, I created a pipeline that trains a model on the Bank Marketing dataset and deploys the model to a REST endpoint that can be consumed by other applications. 

## Architectural Diagram
![Steps](https://github.com/DrewAumick/Udacity_AZMLND_C2/blob/master/starter_files/Udacity%20class%20screenshots/step%20diagram.png)
For this project, there are 8 steps:
1. Authentication
2. Automated ML Experiment
3. Deploy the best model
4. Enable logging
5. Swagger Documentation
6. Consume model endpoints
7. Create and publish a pipeline
8. Documentation

## Key Steps
### Authentication
Since I used the Udacity provided lab, this step was taken care of for me by the lab. But in essence, it ensures that the Azure Machine Learning Extention (az) command line interface is allowed to communicate with the Azure ML Studio

### Automate ML Experiment
For this step, first I had to make sure the Bank Marketing dataset was uploaded to the Azure ML Datastore

![Datasets](https://github.com/DrewAumick/Udacity_AZMLND_C2/blob/master/starter_files/Udacity%20class%20screenshots/Registered%20Datasets.PNG)

Next, I created an AutoML Classification experiment and ran it to find the best model.

![Completed Experiment](https://github.com/DrewAumick/Udacity_AZMLND_C2/blob/master/starter_files/Udacity%20class%20screenshots/Complete%20Experiment.PNG)

Then, I identified the best model from this AutoML experiment, which turned out to be a VotingEnsemble model

![Best Model](https://github.com/DrewAumick/Udacity_AZMLND_C2/blob/master/starter_files/Udacity%20class%20screenshots/Best%20Experiment.PNG)

### Deploy the Best Model
Now that the best model was identified, I deployed it as an endpoint.

![Deployed Endpoint](https://github.com/DrewAumick/Udacity_AZMLND_C2/blob/master/starter_files/Udacity%20class%20screenshots/Endpoint%20Details.PNG)

### Enable Logging
To enable logging, I turned on the 'Application Insights' for the deployed endpoint and ran a script to view the log output

![Application Insights On](https://github.com/DrewAumick/Udacity_AZMLND_C2/blob/master/starter_files/Udacity%20class%20screenshots/Application%20Insights%20enabled.PNG)

![Log output](https://github.com/DrewAumick/Udacity_AZMLND_C2/blob/master/starter_files/Udacity%20class%20screenshots/logs.PNG)

### Swagger Documentation
Next, I deployed a Swagger UI docker container and a simple server to be able to display the swagger documentation for this endpoint

![Swagger UI](https://github.com/DrewAumick/Udacity_AZMLND_C2/blob/master/starter_files/Udacity%20class%20screenshots/swagger%20ui.PNG)

### Consume Model Endpoint
Next, I ran a script to ensure that the endpoint could actually be consumed by an application and ran a benchmark to ensure it runs efficiently.

![Endpoint Output](https://github.com/DrewAumick/Udacity_AZMLND_C2/blob/master/starter_files/Udacity%20class%20screenshots/endpoint%20output.PNG)

![Benchmark Output1](https://github.com/DrewAumick/Udacity_AZMLND_C2/blob/master/starter_files/Udacity%20class%20screenshots/benchmark%20output%201.PNG)
![Benchmark Output2](https://github.com/DrewAumick/Udacity_AZMLND_C2/blob/master/starter_files/Udacity%20class%20screenshots/benchmark%20output%202.PNG)

### Create and publish a pipeline
Next I used a [Jupyter Notebook](https://github.com/DrewAumick/Udacity_AZMLND_C2/blob/master/starter_files/aml-pipelines-with-automated-machine-learning-step.ipynb) to create and publish a pipeline. 

![Pipelines](https://github.com/DrewAumick/Udacity_AZMLND_C2/blob/master/starter_files/Udacity%20class%20screenshots/Pipelines.PNG)

![Pipeline Endpoints](https://github.com/DrewAumick/Udacity_AZMLND_C2/blob/master/starter_files/Udacity%20class%20screenshots/Pipeline%20Endpoints.PNG)

![Published Pipeline](https://github.com/DrewAumick/Udacity_AZMLND_C2/blob/master/starter_files/Udacity%20class%20screenshots/Published%20Pipeline.PNG)

![ML Studio Run](https://github.com/DrewAumick/Udacity_AZMLND_C2/blob/master/starter_files/Udacity%20class%20screenshots/ML%20studio%20scheduled%20run.PNG)

![RunDetails Run](https://github.com/DrewAumick/Udacity_AZMLND_C2/blob/master/starter_files/Udacity%20class%20screenshots/RunDetails.PNG)

### Documentation
Finally, I created documentation in the form of this README file and the screencast video below.

## Screen Recording
Click the link below to see a screencast I did demonstrating the completed artifacts from my project.
https://youtu.be/X7P__8LAbBo

