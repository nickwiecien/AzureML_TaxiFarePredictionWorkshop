# Azure Machine Learning - NYC Taxi Cab Fare Prediction Workshop

## Overview
This workshop showcases how to take data from a CSV, train a custom model using AutoML inside of Azure Machine Learning, deploy that model to a real-time endpoint, and consume the predictions through a custom Power BI report. 

![NYC Taxi Cab Fare Prediction Workshop](doc_img/yellow_taxi.jpg?raw=true "NYC Taxi Cab Fare Prediction Workshop")

In this workshop we will be using taxi data from the open-source NYC Yellow Taxi Cab Dataset. The data located in `taxi_data/training_data.csv` and `taxi_data/testing_data.csv` has been retrieved from the Azure Open Dataset and formatted to include the following fields:

| Column Name | Column Description | 
|-------------|--------------------|
|PICKUP_LOCATION_CODE| A unique code mapping to a geo location of a taxi cab pickup |
|DROPOFF_LOCATION_CODE| A unique code mapping to a geo location of a taxi cab dropoff |
|PICKUP_YEAR| The year in which the cab ride was initiated |
|PICKUP_MONTH| The month in which the cab ride was initiated |
|PICKUP_DAY| The date in which the cab ride was initiated |
|PICKUP_HOUR| The hour in which the cab ride was initiated |
|PICKUP_MINUTE| The minute in which the cab ride was initiated |
|RATE_CODE| Indicates the rate for the taxi cab ride (group, airport, etc.) |
|TRIP_DISTANCE| The total distance of the cab ride in miles |
|TOTAL_AMOUNT| The total amount paid by the rider (fare + tip). Used as an output in this workshop |

With the data included in this repo, we aim to train and deploy a custom machine learning model which can predict the total amount a rider will pay out for a taxi ride, based on the inputs listed above. We will then integrate this model into a custom Power BI dashboard to make predictions about fares for future cab rides.

## Getting Started

The guided walk through below highlights how to provision a new Azure Machine Learning workspace, create necessary compute resources in the AML workspace, upload a training dataset, start an AutoML model training job, deploy the trained model to an endpoint, and then consume that model through a custom Power BI Dashboard.

### Step 1 - Download and Extract GitHub Repository to your Local Machine

First, download this repository with the underlying data for training and validation to your local machine. Click on the <i><> Code</i> dropdown link and select 'Download ZIP'.

![Download ZIP](doc_img/01.png?raw=true "Download ZIP")

Once completed you should see a zipped directory named 'AzureML_TaxiFarePredictionWorkshop-main' in your Downloads folder. Extract the zipped directory to a known location on your computer.

![Extract ZIP](doc_img/02.png?raw=true "Extract ZIP")

Navigate to the extracted directory and click into the `taxi_data` directory. You should see two files: `testing_data.csv` and `training_data.csv`. These will be used for model training and evaluation later on in the workshop.

![Test and Train Taxi Data](doc_img/03.png?raw=true "Test and Train Taxi Data")

### Step 2 - Provision an Azure Machine Learning Workspace
 
Sign in to the (Azure Portal)[https://ms.portal.azure.com] and navigate to your target resource group for the workshop exercise. From the home page you can type <i>Resource groups</i> into the top search bar and select the associated option under the services menu.

![Resource groups](doc_img/04.png?raw=true "Resource groups")

From your resource group list, filter to your target resource group by name, and click into the group. <b>Note:</b> your resource group will not be named the same as is shown in the directory. Reach out to your workshop facilitators if you have questions about what resource group you should use.

![Target Resource Group](doc_img/05.png?raw=true "Target Resource Group")

Inside your resource group, click the <i>+ Create</i> button at the top of the page to provision new resources.

![Create Resources](doc_img/06.png?raw=true "Create Resources")

Clicking <i>+ Create</i> will take you to the Azure marketplace. In the <i>Search services and marketplace</i> bar enter 'Machine Learning' and select the matching option.

![Search Machine Learning](doc_img/07.png?raw=true "Search Machine Learning")

You should see an offer for a new Azure Machine Learning Workspace from Microsoft as shown below. Click the <i>Create</i> button to start provisioning your workspace.

![Create Azure Machine Learning Workspace](doc_img/08.png?raw=true "Create Azure Machine Learning Workspace")

From the Azure ML resource creation form, select your target subscription and resource group. Under the <i>Workspace name</i> field, we recommend naming your workspace something which includes your name.  <b>Note:</b> Azure machine learning workspace names need to be globally unique. Next, select a target region - we recommend East US 2. Azure machine learning automatically creates an Azure Storage Account, an Azure Container Registry, and an instance of Azure Key Vault. Collectively these services are leveraged to support creation of ML assets. We recommend leaving the autofilled names as they are as you will not need to interact with these resources directly, rather through the AML workspace. Once all fields have been filled click <i>Review + create</i>. Default values on the Networking, Advanced, and Tags tab can be left as is.

| Form Field | Value |
|------------|-------|
|Subscription | Target Azure subscription used for the workshop|
|Resource group | Target Azure resource group used for the workshop - contained within the target subscription|
|Workspace name | Provide a name that uses the following pattern '<YOUR FIRST NAME>-<YOUR LAST NAME>-aml-ws' |
|Region | Recommended:  East US 2|
|Storage Account| Accept autofilled value |
|Key Vault | Accept autofilled value |
|Application Insights | Accept autofilled value |
|Container Registry | None|

![Azure Machine Learning Creation Form](doc_img/09.png?raw=true "Azure Machine Learning Creation Form")

On the review and create tab, once the validation has passed click the <i>Create</i> button.

![Azure Machine Learning Create Tab](doc_img/10.png?raw=true "Azure Machine Learning Create Tab")

After clicking <i>Create</i> a new Azure Machine Learning deployment will start, a process which may take a few minutes. Once the deployment is complete, click the <i>Go to resource</i> button.

![Azure Machine Learning Go-To Resource](doc_img/11.png?raw=true "Azure Machine Learning Go-To Resource")

From the Azure Machine Learning resource page in the Azure ML portal, click the <i>Launch studio</i> button in the center of the page.

![Azure Machine Learning Launch Studio](doc_img/12.png?raw=true "Azure Machine Learning Launch Studio")

A new browser tab containing the Azure Machine Learning Studio UI should open.

![Azure Machine Learning Studio](doc_img/13.png?raw=true "Azure Machine Learning Studio")

### Step 3 - Register Taxi Training Data as Azure Machine Learning Dataset

Upload CSV into workspace

### Step 4 - Create an Azure Machine Learning Compute Cluster to Support Model Training

Create single node cluster

### Step 5 - Create and Run an AutoML Job to Train a Fare Prediction Model

Submit AutoML job with appropriate target column - timeout limit of 30 minutes

### Step 6 - Deploy your Trained Model to an Azure Container Instance

Deploy to web service and verify prediction through testing portal once complete

### Step 7 - Load PBIX Template in Power BI Desktop

### Step 8 - Connect Deployed Model into Power BI Report

