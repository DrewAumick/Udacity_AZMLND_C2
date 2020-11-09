# Operationalizing Machine Learning Project

In this project, I use AutoML to train lots of models on the Bank Marketing dataset and deploy the best one to a REST endpoint that can be consumed by other applications. I then create a pipeline using the Azure SDK that will automate the model training process. This pipeline is itself deployed to an endpoint so it can be started with a simple REST API call. 

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
For this step, first I had to make sure the Bank Marketing dataset was uploaded to the Azure ML Datastore using the Azure ML Datasets interface.

![Datasets](https://github.com/DrewAumick/Udacity_AZMLND_C2/blob/master/starter_files/Udacity%20class%20screenshots/Registered%20Datasets.PNG)

Next, I created an AutoML Classification experiment using the Azure ML AutoML UI. This experiment trained several dozen different models at the same time on an Azure compute cluster allowing me to find a model with high accuracy in a relatively short amount of time.

![Completed Experiment](https://github.com/DrewAumick/Udacity_AZMLND_C2/blob/master/starter_files/Udacity%20class%20screenshots/Complete%20Experiment.PNG)

Then, I identified the best model from this AutoML experiment, which turned out to be a VotingEnsemble model. This makes sense since Voting Ensembles take the output from several different types of classification modles and gives them a weighted vote to get the classifaction decision.

![Best Model](https://github.com/DrewAumick/Udacity_AZMLND_C2/blob/master/starter_files/Udacity%20class%20screenshots/Best%20Experiment.PNG)

### Deploy the Best Model <a name="best_model" />
Now that the best model was identified, I deployed it as a REST endpoint by simply clicking the 'Deploy' button in the Azure ML Studio from the Model details UI.

![Deployed Endpoint](https://github.com/DrewAumick/Udacity_AZMLND_C2/blob/master/starter_files/Udacity%20class%20screenshots/Endpoint%20Details.PNG)

### Enable Logging <a name="logging" />
To enable logging, I use a python [logging script](https://github.com/DrewAumick/Udacity_AZMLND_C2/blob/master/starter_files/logs.py) that turned on the 'Application Insights' for the deployed endpoint and output the logs. In the first screenshot below, you can see the endpoint shows 'Application Insights enabled' is true, and the second shows the log output. These logs let you see details about how well the endpoint is running.

![Application Insights On](https://github.com/DrewAumick/Udacity_AZMLND_C2/blob/master/starter_files/Udacity%20class%20screenshots/Application%20Insights%20enabled.PNG)

![Log output](https://github.com/DrewAumick/Udacity_AZMLND_C2/blob/master/starter_files/Udacity%20class%20screenshots/logs.PNG)

### Swagger Documentation <a name="swagger" />
Next, I deployed a Swagger UI docker container and a simple server to be able to display the swagger documentation for this endpoint using the [swagger.sh](https://github.com/DrewAumick/Udacity_AZMLND_C2/blob/master/starter_files/swagger/swagger.sh) and [serve.py](https://github.com/DrewAumick/Udacity_AZMLND_C2/blob/master/starter_files/swagger/serve.py) scripts. We can use this UI to see what GETs and POSTs can be performed on the endpoint. In addition, it allows you to see the correct JSON format for the POST if we want to get inference results from our model. You can see exactly what fields are expected and get an idea for the data type of each field (ie dates, strings, ints, etc).

![Swagger UI](https://github.com/DrewAumick/Udacity_AZMLND_C2/blob/master/starter_files/Udacity%20class%20screenshots/swagger%20ui.PNG)

### Consume Model Endpoint <a name="consume" />
Next, I ran a simple [python script](https://github.com/DrewAumick/Udacity_AZMLND_C2/blob/master/starter_files/endpoint.py) using the data format shown in Swagger to ensure that the endpoint could actually be consumed by an application. Below we can see the response that the endpoint returned

![Endpoint Output](https://github.com/DrewAumick/Udacity_AZMLND_C2/blob/master/starter_files/Udacity%20class%20screenshots/endpoint%20output.PNG)

Next I used Apache Benchmark to run a benchmark against the endpoint to ensure it runs efficiently. We can see in the benchmark output that none of the 10 requests failed and that they all came back extremely quickly.

![Benchmark Output1](https://github.com/DrewAumick/Udacity_AZMLND_C2/blob/master/starter_files/Udacity%20class%20screenshots/benchmark%20output%201.PNG)
![Benchmark Output2](https://github.com/DrewAumick/Udacity_AZMLND_C2/blob/master/starter_files/Udacity%20class%20screenshots/benchmark%20output%202.PNG)

### Create and publish a pipeline <a name="publish" />
Next I used a [Jupyter Notebook](https://github.com/DrewAumick/Udacity_AZMLND_C2/blob/master/starter_files/aml-pipelines-with-automated-machine-learning-step.ipynb) to create and publish the whole pipeline. In the notebook, we first import the various libraries and packages we'll need, mostly from the Azure SDK. Then we connect to the Azure Workspace and search for a compute cluster, creating one if it doesn't exist. We also check to see if the Bankmarketing dataset is uploaded and upload it if it isn't. Then we configure an AutoML Step which will perform an AutoML search for an optimal model as described in [Section 2](#automl). The full pipeline is then set up using this AutoML step with the dataset being fed into it. The notebook runs the pipeline and does some testing of model results to ensure it's working.

Here we see the pipeline:
![Pipelines](https://github.com/DrewAumick/Udacity_AZMLND_C2/blob/master/starter_files/Udacity%20class%20screenshots/Pipelines.PNG)

After the pipeline has been created, we use the Azure SDK and our notebook to publish it to a REST endpoint. This will allow us to run the pipeline using a simple REST API call.

The published endpoint:
![Pipeline Endpoints](https://github.com/DrewAumick/Udacity_AZMLND_C2/blob/master/starter_files/Udacity%20class%20screenshots/Pipeline%20Endpoints.PNG)

Details of the published pipeline endpoint, including the REST URL:
![Published Pipeline](https://github.com/DrewAumick/Udacity_AZMLND_C2/blob/master/starter_files/Udacity%20class%20screenshots/Published%20Pipeline.PNG)

Lastly, in the notebook we make a REST call using a POST to make sure the pipeline kicks off as we expect it to. Once it's running, we can check in on the run's progress both through the Azure ML Studio UI and the RunDetails widget in our notebook.

A view of the pipeline running in Azure ML Studio:
![ML Studio Run](https://github.com/DrewAumick/Udacity_AZMLND_C2/blob/master/starter_files/Udacity%20class%20screenshots/ML%20studio%20scheduled%20run.PNG)

And a view of the pipeline running from the RunDetails widget in Jupyter Notebook:
![RunDetails Run](https://github.com/DrewAumick/Udacity_AZMLND_C2/blob/master/starter_files/Udacity%20class%20screenshots/RunDetails.PNG)

### Documentation <a name="docs">
Finally, I created documentation in the form of this README file and a screencast video. Click [here](https://youtu.be/X7P__8LAbBo) to see the screencast I did demonstrating the completed artifacts from my project.
  
## Suggestions for Future Work 
For future work, I would suggest making further tweaks to the AutoML step of the pipeline. There are a lot of settings involved and making changes to them could improve the search space and help find an even better model solution. There are also additional steps that could be added to the pipeline, perhaps doing some dataset cleanup or feature engineering before the AutoML step, or doing additional steps after the AutoML step has completed.

