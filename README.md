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