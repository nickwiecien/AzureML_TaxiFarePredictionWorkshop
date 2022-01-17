# Azure Machine Learning - NYC Taxi Cab Fare Prediction Workshop

## Overview
This workshop showcases how to train, deploy, and consume a custom model using Azure resources. Specifically, this workshop walks through how to load data into [Azure Machine Learning](https://azure.microsoft.com/en-us/services/machine-learning/) from a local CSV, train a custom model using [AutoML](https://docs.microsoft.com/en-us/azure/machine-learning/concept-automated-ml), deploy that model to a real-time endpoint, and consume the predictions through a custom [Power BI](https://powerbi.microsoft.com/en-us/) report. 

After completing this workshop, you will be prepared to train, deploy, and consume custom ML models built on top of your own data without having to write any code whatsoever!

![NYC Taxi Cab Fare Prediction Workshop](doc_img/yellow_taxi.jpg?raw=true "NYC Taxi Cab Fare Prediction Workshop")

In this workshop we will be using taxi data from the [open-source NYC Yellow Taxi Cab Dataset](https://docs.microsoft.com/en-us/azure/open-datasets/dataset-taxi-yellow?tabs=azureml-opendatasets). The data located in `taxi_data/training_data.csv` and `taxi_data/testing_data.csv` has been retrieved from the Azure Open Dataset and formatted to include the following fields:

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

With the data included in this repo, we aim to train and deploy a custom machine learning model which can <b>predict the total amount a rider will pay for a taxi ride</b>, based on the inputs listed above. We will then integrate this model into a custom Power BI dashboard to make predictions about fares for future cab rides.

## Getting Started

The guided walkthrough below highlights how to provision a new Azure Machine Learning workspace, create necessary compute resources in the workspace, upload a training dataset, launch an AutoML model training job, deploy the trained model to an endpoint, and then consume that model through a custom Power BI report.

- [Step 1 - Download and Extract GitHub Repository to your Local Machine](#step-1---download-and-extract-github-repository-to-your-local-machine)
 - [Step 2 - Provision an Azure Machine Learning Workspace](#step-2---provision-an-azure-machine-learning-workspace)
 - [Step 3 - Register Taxi Training Data as Azure Machine Learning Dataset](#step-3---register-taxi-training-data-as-azure-machine-learning-dataset)
 - [Step 4 - Create an Azure Machine Learning Compute Cluster to Support Model Training](#step-4---create-an-azure-machine-learning-compute-cluster-to-support-model-training)
 - [Step 5 - Create and Run an AutoML Job to Train a Fare Prediction Model](#step-5---create-and-run-an-automl-job-to-train-a-fare-prediction-model)
 - [Step 6 - Deploy your Trained Model to an Azure Container Instance](#step-6---deploy-your-trained-model-to-an-azure-container-instance)
 - [Step 7 - Create New Report in Power BI Desktop](#step-7---create-new-report-in-power-bi-desktop)
 - [Step 8 - Connect Deployed Model into Power BI Report](#step-8---connect-deployed-model-into-power-bi-report)
 - [Next Steps and References](#next-steps-and-references)

## Step 1 - Download and Extract GitHub Repository to your Local Machine

First, download this repository with the underlying data for training and validation to your local machine. Click on the <i><> Code</i> dropdown link and select 'Download ZIP'.

![Download ZIP](doc_img/01.png?raw=true "Download ZIP")

Once completed you should see a zipped directory named 'AzureML_TaxiFarePredictionWorkshop-main' in your Downloads folder. Extract the zipped directory to a known location on your computer.

![Extract ZIP](doc_img/02.png?raw=true "Extract ZIP")

Navigate to the extracted directory and click into the `taxi_data` directory. You should see two files: `testing_data.csv` and `training_data.csv`. These will be used for model training and evaluation later on in the workshop.

![Test and Train Taxi Data](doc_img/03.png?raw=true "Test and Train Taxi Data")

## Step 2 - Provision an Azure Machine Learning Workspace
 
Sign in to the [Azure Portal](https://ms.portal.azure.com) and navigate to your target resource group for the workshop exercise. From the home page you can type <i>Resource groups</i> into the top search bar and select the associated option under the services menu.

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
|Workspace name | Provide a name that uses the following pattern 'FIRSTNAME-LASTNAME-aml-ws' |
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

## Step 3 - Register Taxi Training Data as Azure Machine Learning Dataset

Inside the AML studio, we need to upload our taxi training data before kicking off a model training job. From the Azure Machine Learning Studio, click <i>Datasets</i> in the left rail.

![Datasets](doc_img/14.png?raw=true "Datasets")

Once inside the Datasets tab, click the <i>Create dataset</i> dropdown in the center of the page and select <i>From local files</i>.

![Dataset from Local Files](doc_img/15.png?raw=true "Dataset from Local Files")

Under 'Basic Info' enter `Taxi_Training_Data` as your dataset name and leave the dataset type as `Tabular`. After entering these fields click <i>Next</i>.

| Field | Value |
|-------|-------|
|Name | Taxi_Training_Data |
|Dataset type | Tabular |
|Description | New York City yellow taxi fares data |

![Dataset Basic Info](doc_img/16.png?raw=true "Dataset Basic Info")

Under 'Datastore and file selection' leave the default datastore selection as `workspaceblobstore`. Click the <i>Upload</i> dropdown and navigate to the `taxi_data/training_data.csv` file you downloaded to your local machine earlier. Select this file and click <i>Open</i>. Leave 'Skip data validation' unchecked and click <i>Next</i>. This will trigger your file upload.

![Datastore and File Selection](doc_img/17.png?raw=true "Datastore and File Selection")

From the 'Settings and preview' tab, leave all of the default options selected. Your screen should appear as what is shown below. After confirming your data has been uploaded as expected, click  <i>Next</i>.

![Settings and Preview](doc_img/18.png?raw=true "Settings and Preview")

Under the 'Schema' tab all of the columns in the dataset will be auto-type detected. Leave all of the columns on and default settings selected. Click <i>Next</i>.

![Schema](doc_img/19.png?raw=true "Schema")

Finally, from the 'Confirm details' tab, click <i>Create</i>. Your dataset is now registered (saved and versioned) and can be consumed from the AML workspace and used to support model training.

![Confirm Details](doc_img/20.png?raw=true "Confirm Details")

## Step 4 - Create an Azure Machine Learning Compute Cluster to Support Model Training

Now, to support our AutoML training job to be launched in the next step of the workshop, we need to create compute resources to handle the training activities. First, navigate to the <i>Compute</i> tab along the left rail.

![Compute](doc_img/21.png?raw=true "Compute")

From the Compute panel select the <i>Compute clusters</i> tab, and then click the <i>+ New</i> button.

![Compute Clusters](doc_img/22.png?raw=true "Compute Clusters")

From the 'Virtual Machine' tab, leave the default values selected. The `Standard_DS3_v2` VM will work well for the upcoming training operation. For reference, you can optionally select larger virtual machines (including GPU machines) to support more intensive model training operations (deep learning, vision, etc.). Click <i>Next</i>.

![Virtual Machines](doc_img/23.png?raw=true "Virtual Machines")

In  the 'Advanced Settings' panel, provide a name for your compute cluster (recommend using something containing your initials), and leave the minimum node count, maximum node count, and idle seconds before scale down settings to their defaults. Leave 'Enable SSH access' unchecked and do not modify the Advanced settings. After entering all fields, click <i>Create</i>.

| Field | Value |
|-------|-------|
|Compute name | YOURINITIALS-training |
|Minimum number of nodes | 0 |
| Maximum number of nodes | 1 |
|Idle seconds before scale down | 120 |

![Compute Cluster Advanced Settings](doc_img/24.png?raw=true "Compute Cluster Advanced Settings")

Your compute cluster will automatically begin provisioning and once successful show indicate 'Succeeded (0 nodes)'. 

<b>Note:</b> Your nodes will automatically spin up when jobs are submitted, and will spin down following 120 seconds of inactivity, and you are only billed for consumption while these nodes are active. Keeping the 'Idle seconds before scale down' setting low on your compute cluster will help keep training costs low.

![Provisioned Compute Cluster](doc_img/25.png?raw=true "Provisioned Compute Cluster")

## Step 5 - Create and Run an AutoML Job to Train a Fare Prediction Model

After registering your taxi cab training dataset and provisioning your training compute resources, click the 'Automated ML' option from the left rail.

![Automated ML](doc_img/26.png?raw=true "Automated ML")

From the Automated ML tab, click <i>+ New Automated ML run</i>.

<b>Note:</b> More details about how AutoML works under the hood, and what applications it is well-suited towards can be found under the 'Concept: What is Automated ML?' tab.

![New Automated ML Run](doc_img/27.png?raw=true "New Automated ML Run")

When prompted to select a dataset, choose the `Taxi_Training_Data` dataset that you previously registered in the AML workspace, then click <i>Next</i>

![Select AutoML Dataset](doc_img/28.png?raw=true "Select AutoML Dataset")

Under the 'Configure Run' tab, populate the fields according to the table below, the click <i>Next</i>.

| Field | Value |
|-------|-------|
|New experiment name | Taxi_Fare_Prediction|
|Target column | TOTAL_AMOUNT (Decimal) |
|Select compute type | Compute cluster |
|Select Azure ML compute cluster | <i>Select the compute cluster you provisioned in step 4. There should be one available option here. </i>|

![Configure AutoML Run](doc_img/29.png?raw=true "Configure AutoML Run")

From the 'Select Task and Settings' panel, select <i>Regression</i>, then click the <i>View additional configuration settings</i> button.

![AutoML Task and Settings](doc_img/30.png?raw=true "AutoML Task and Settings")

Under the 'Additional configurations' tab, expand the 'Exit criterion' tab and change <i>Training job time (hours)</i> setting to `0.5`. Once updated click <i>Save</i>. From the 'Select task and settings' menu, click <i>Next</i>.

<b>Note:</b> For the purposes of this workshop we want our model training to complete in a timely manner. For other applications, extended training times can often yield higher performing models.

![AutoML Additional Configuration](doc_img/31.png?raw=true "AutoML Additional Configuration")

Under the '[Optional] Validate and Test' menu, leave the defaults selected and click <i>Finish</i>.

![Finalize AutoML Settings](doc_img/32.png?raw=true "Finalize AutoML Settings")

Your AutoML training job should start immediately. Now would be a great time to go grab a coffee as this next part will take ~20-30 minutes ☕🕒. 

After your run has completed it should reflect the best performing model under the 'Best model summary'. You can review all trained models under the 'Models' tab.

![AutoML Completion](doc_img/33.png?raw=true "AutoML Completion")

## Step 6 - Deploy your Trained Model to an Azure Container Instance

Now we will deploy your best performing model to a real-time endpoint so that it can be consumed by other applications and services. Click the hyperlink to your best performing model.

![Best Performing Model](doc_img/33b.png?raw=true "Best Performing Model")

<u><b>IMPORTANT!</b> In order to deploy a Power BI-compatible endpoint the following steps are critical in your model deployment.</u>

From the overview of your best performing model. navigate to the 'Outputs + logs' tab. Under the `outputs` directory in the file explorer click the ellipsis (...) next to `outputs/conda_env_v_1_0_0.yml` and `outputs/scoring_file_v_1_0_0.py` to download both to your local machine. It is important to download the `scoring_file_v_1_0_0.py` version.

![Download Model Env and Scoring File](doc_img/34.png?raw=true "Download Model Env and Scoring File")

Once you have downloaded these two files, click the <i>Deploy</i> dropdown menu and select <i>Deploy to web service</i>.

![Model Web Service Deployment](doc_img/35.png?raw=true "Model Web Service Deployment")

Populate the 'Deploy a Model' tab with the information in the table below. Note the `scoring_file_v_1_0_0.py` and `conda_env_v_1_0_0.yml` should be sourced from your local machine. After entering all fields, click  <i>Deploy</i>.

| Field | Value |
|-------|-------|
|Name | fare-prediction-deployment-YOURINITIALS |
|Compute type | Azure Container Instance |
|Enable authentication | <i>Leave unchecked</i> |
|Use custom deployment assets | TRUE |
|Entry script file | `scoring_file_v_1_0_0.py` |
|Conda dependencies file | `conda_env_v_1_0_0.yml` |

![Deploy a Model](doc_img/36.png?raw=true "Deploy a Model")

Your model endpoint deployment should begin automatically and this process can take a few minutes (~10-15) to complete. If you do not already have Power BI Desktop installed, move ahead to Step 7 to complete the installation.

You can monitor your deployment by clicking on the 'Endpoints' tab on the left rail, and clicking into your <i>fare-prediction-deployment</i> endpoint. Once complete, your Deployment state should indicate 'Healthy'.

![Successful Deployment](doc_img/37.png?raw=true "Sucessful Deployment")

You can test your endpoint by navigating to the <i>Test</i> tab. Select the <i>JSON editor</i> option and enter the JSON below then click <i>Test</i>. Select 'raw' under the Test result panel. <b>Note:</b> Your specific results will likely be slightly different.
```
{
  "data": [
    {
      "DROPOFF_LOCATION_CODE": "Loc_113",
      "PICKUP_DAY": 6,
      "PICKUP_HOUR": 16,
      "PICKUP_LOCATION_CODE": "Loc_79",
      "PICKUP_MINUTE": 4,
      "PICKUP_MONTH": 4,
      "PICKUP_YEAR": 2019,
      "RATE_CODE": "GroupRide",
      "TRIP_DISTANCE": 0.57,
      "VENDOR_ID": "VeriFone Inc."
    }
  ]
}
```

![Model Test](doc_img/38.png?raw=true "Model Test")

## Step 7 - Create New Report in Power BI Desktop

If you do not have Power BI Desktop installed, [download the PBI Desktop Installer from the Microsoft Download Center](https://www.microsoft.com/en-us/download/details.aspx?id=58494) and install on your local machine.

Launch Power BI Desktop from the start menu.

![Launch Power BI](doc_img/39.png?raw=true "Launch Power BI")

Once Power BI has opened on desktop click the <i>Get data</i> drop down from the top ribbon and select <i>Text/CSV</i>.

![Load Text/CSV](doc_img/40.png?raw=true "Load Text/CSV")

From the file explorer, navigate to the `taxi_data/testing_data.csv` file you downloaded to your local machine earlier in the workshop. Select the file and click <i>Open</i>.

![Load Testing Data](doc_img/41.png?raw=true "Load Testing Data")

A data preview panel will appear with similar data to what was used to train your fare prediction model. Click the <i>Transform Data</i> button. In the next section we will connect to your deployed model and create an interactive report visualization.

![Transform Data](doc_img/42.png?raw=true "Transform Data")

## Step 8 - Connect Deployed Model into Power BI Report

From the Power Query Editor, click the <i>Azure Machine Learning</i> option and sign into your Azure AD account when prompted.

![Power BI Azure ML](doc_img/43.png?raw=true "Power BI Azure ML")

From the 'Azure Machine Learning Models' dialog, your deployed model should appear with the name `AzureML.fare-prediction-deployment-YOURINITIALS`. Click this option. The fields should be automatically populated with appropriate columns from the testing dataset. Then click <i>OK</i>.

![Azure ML Fare Prediction Model](doc_img/44.png?raw=true "Azure ML Fare Prediction Model")

When prompted choose to ignore the privacy level details. No sensitive information is being used in this workshop.

![Azure ML Privacy](doc_img/45.png?raw=true "Azure ML Privacy")

A new column should appear with predictions from your deployed model. Rename this new column to `PREDICTED_AMOUNT` by modifying the name in the formula bar at the top of the page.

![Predicted Amount Column](doc_img/46.png?raw=true "Predicted Amount Column")

Next, we need to confirm that both the <i>TOTAL_AMOUNT</i> and <i>PREDICTED_AMOUNT</i> columns reflect the appropriate numeric type. Click the 'Transform' tab along the top ribbon, select each column one at a time, and update the data type to <i>Decimal Number</i>. The icon attached to each column should appear as <b>1.2</b>.

![Update Column Types](doc_img/47.png?raw=true "Update Column Types")

To save your changes, navigate back to the 'Home' tab in the report and click `Close & Apply`.

![Close & Apply](doc_img/48.png?raw=true "Close & Apply")

From the report add a new visualization. From the visualization panel, double-click the <i>Scatter Plot</i> option in the center of the panel. This will automatically add a new visualization to your report which you can dynamically resize.

![Add Scatter Plot](doc_img/49.png?raw=true "Add Scatter Plot")

Add data to your visualization. Drag the 'TOTAL_AMOUNT' column under the 'X Axis' field, and the 'PREDICTED_AMOUNT' column under the 'Y Axis'. For both, click the drop-down menu and select <i>Don't summarize</i>.

![Scatter Plot](doc_img/50.png?raw=true "Scatter Plot")

Your plot should appear similar to what is shown here with substantial density along the identity line with some outliers falling well off of it. You can also add fields to the 'Tooltips' section. This will allow you to hover over points and view additional details about the datapoint.

![Tooltips](doc_img/51.png?raw=true "Tooltips")

Congratulations on finishing the workshop🙏! Feel free to continue creating new visualizations in Power BI using predictions sourced from your deployed model. Be sure to delete all created resources at the conclusion of the workshop unless you want to use these as a reference in the future. You can delete resources by navigating to the Azure portal, selecting the Azure ML workspace and clicking delete.

![Delete AML Workspace](doc_img/52.png?raw=true "Delete AML Workspace")

## Next Steps and References

We hope you enjoyed this workshop! If you are interested in using Azure Machine Learning for future model development activities, the resources linked below are a great place to get started on learning what else can be done with AML!

* [Microsoft Learn (Self-Guided Learning Path) - Build and Operate Machine Learning Solutions with Azure Machine Learning](https://docs.microsoft.com/en-us/learn/paths/build-ai-solutions-with-azure-ml-service/)

* [Microsoft Documentation - What is Azure Machine Learning?](https://docs.microsoft.com/EN-US/azure/machine-learning/overview-what-is-azure-machine-learning)

* [Azure Machine Learning Examples (GitHub)](https://github.com/Azure/azureml-examples)

* [Azure Machine Learning Data Scientist Associate Certification (DP-100)](https://docs.microsoft.com/en-us/learn/certifications/azure-data-scientist/)
