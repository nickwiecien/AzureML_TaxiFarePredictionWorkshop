# Azure Machine Learning - NYC Taxi Cab Fare Prediction Workshop

<b>Overview:</b> This workshop showcases how to take data from a CSV, train a custom model using AutoML inside of Azure Machine Learning, deploy that model to a real-time endpoint, and consume the predictions through a custom Power BI report. 

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
|TOTAL_AMOUNT| The total amount paid by the rider (fare + tip) - used as an output here |

With the data included in this repo, we aim to train and deploy a custom machine learning model which can predict the total amount a rider will pay out for a taxi ride, based on the inputs listed above.

