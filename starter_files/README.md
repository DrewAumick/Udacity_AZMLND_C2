# Operationalizing Machine Learning Project

In this project, I created a pipeline that trains a model on the Bank Marketing dataset and deploys the model to a REST endpoint that can be consumed by other applications. This pipeline is itself deployed to an endpoint so it can be started with a simple REST API call. 

## Architectural Diagram
![Steps](https://github.com/DrewAumick/Udacity_AZMLND_C2/blob/master/starter_files/Udacity%20class%20screenshots/step%20diagram.png)
For this project, there are 8 steps:
1. [Authentication](#auth)
2. [Automated ML Experiment](#automl)
3. [Deploy the best model](#best_model)
4. [Enable logging](#logging)
5. [Swagger Documentation](#swagger)
6. [Consume model endpoints](#consume)
7. [Create and publish a pipeline](#publish)
8. [Documentation](#docs)

## Key Steps

### Authentication <a name="auth" />
Since I used the Udacity provided lab, this step was taken care of for me by the lab. But in essence, it ensures that the Azure Machine Learning Extention (az) command line interface is allowed to communicate with the Azure ML Studio.

### Automate ML Experiment <a name="automl" />
For this step, first I had to make sure the Bank Marketing dataset was uploaded to the Azure ML Datastore

![Datasets](https://github.com/DrewAumick/Udacity_AZMLND_C2/blob/master/starter_files/Udacity%20class%20screenshots/Registered%20Datasets.PNG)

Next, I created an AutoML Classification experiment and ran it to find the best model.

![Completed Experiment](https://github.com/DrewAumick/Udacity_AZMLND_C2/blob/master/starter_files/Udacity%20class%20screenshots/Complete%20Experiment.PNG)

Then, I identified the best model from this AutoML experiment, which turned out to be a VotingEnsemble model

![Best Model](https://github.com/DrewAumick/Udacity_AZMLND_C2/blob/master/starter_files/Udacity%20class%20screenshots/Best%20Experiment.PNG)

### Deploy the Best Model <a name="best_model" />
Now that the best model was identified, I deployed it as a REST endpoint by simply clicking the 'Deploy' button in the Azure ML Studio.

![Deployed Endpoint](https://github.com/DrewAumick/Udacity_AZMLND_C2/blob/master/starter_files/Udacity%20class%20screenshots/Endpoint%20Details.PNG)

### Enable Logging <a name="logging" />
To enable logging, I use a python logging script that turned on the 'Application Insights' for the deployed endpoint output the logs. In the first screenshot below, you can see the endpoint shows 'Application Insights enabled' is true, and the second shows the log output.

![Application Insights On](https://github.com/DrewAumick/Udacity_AZMLND_C2/blob/master/starter_files/Udacity%20class%20screenshots/Application%20Insights%20enabled.PNG)

![Log output](https://github.com/DrewAumick/Udacity_AZMLND_C2/blob/master/starter_files/Udacity%20class%20screenshots/logs.PNG)

### Swagger Documentation <a name="swagger" />
Next, I deployed a Swagger UI docker container and a simple server to be able to display the swagger documentation for this endpoint. We can use this UI to see what GETs and POSTs can be performed on the endpoint as well as see the correct JSON format for the POST if we want to get inference results from our model.

![Swagger UI](https://github.com/DrewAumick/Udacity_AZMLND_C2/blob/master/starter_files/Udacity%20class%20screenshots/swagger%20ui.PNG)

### Consume Model Endpoint <a name="consume" />
Next, I ran a [python script](https://github.com/DrewAumick/Udacity_AZMLND_C2/blob/master/starter_files/endpoint.py) to ensure that the endpoint could actually be consumed by an application. Below we can see the response that the endpoint returned

![Endpoint Output](https://github.com/DrewAumick/Udacity_AZMLND_C2/blob/master/starter_files/Udacity%20class%20screenshots/endpoint%20output.PNG)

Next I used Apache Benchmark to run a benchmark against the endpoint to ensure it runs efficiently. We can see in the benchmark output that none of the 10 requests failed and that they all came back extremely quickly.

![Benchmark Output1](https://github.com/DrewAumick/Udacity_AZMLND_C2/blob/master/starter_files/Udacity%20class%20screenshots/benchmark%20output%201.PNG)
![Benchmark Output2](https://github.com/DrewAumick/Udacity_AZMLND_C2/blob/master/starter_files/Udacity%20class%20screenshots/benchmark%20output%202.PNG)

### Create and publish a pipeline <a name="publish" />
Next I used a [Jupyter Notebook](https://github.com/DrewAumick/Udacity_AZMLND_C2/blob/master/starter_files/aml-pipelines-with-automated-machine-learning-step.ipynb) to create and publish the whole pipeline. This pipeline will take the Bank Marketing dataset and automatically kick off an AutoML run to train dozens of models and try to find one that can best classify the dataset. After it's been configured and published, the pipeline can be started with a simple REST API call.

Here we see the pipeline:
![Pipelines](https://github.com/DrewAumick/Udacity_AZMLND_C2/blob/master/starter_files/Udacity%20class%20screenshots/Pipelines.PNG)

The published endpoint:
![Pipeline Endpoints](https://github.com/DrewAumick/Udacity_AZMLND_C2/blob/master/starter_files/Udacity%20class%20screenshots/Pipeline%20Endpoints.PNG)

Details of the published pipeline endpoint:
![Published Pipeline](https://github.com/DrewAumick/Udacity_AZMLND_C2/blob/master/starter_files/Udacity%20class%20screenshots/Published%20Pipeline.PNG)

A view of the pipeline running in Azure ML Studio
![ML Studio Run](https://github.com/DrewAumick/Udacity_AZMLND_C2/blob/master/starter_files/Udacity%20class%20screenshots/ML%20studio%20scheduled%20run.PNG)

And a view of the pipeline running from the RunDetails widget in Jupyter Notebook
![RunDetails Run](https://github.com/DrewAumick/Udacity_AZMLND_C2/blob/master/starter_files/Udacity%20class%20screenshots/RunDetails.PNG)

### Documentation <a name="docs">
Finally, I created documentation in the form of this README file and a screencast video. Click [here](https://youtu.be/X7P__8LAbBo) to see the screencast I did demonstrating the completed artifacts from my project.

