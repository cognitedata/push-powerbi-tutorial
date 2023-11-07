# Push data from CDF to Power BI

This repository contains a simple example of how to push data from Cognite Data Fusion to a Power BI `push` or `streaming` dataset. On this example a new `push` dataset was created in Power BI with the following schema:
```json
[
    {
        "timestamp" :"2023-11-07T18:46:22.462Z",
        "value" :98.6,
        "unit" :"AAAAA555555",
        "sensor" :"AAAAA555555",
        "tag" :"AAAAA555555"
    }
]
```

To create a new `push` or `streaming` dataset in **Power BI** follow the steps below:
1. Go to [Power BI](https://app.powerbi.com/) and login with your credentials
2. Go to your workspace and click on `+ New` and then `Streaming dataset`
3. Select `API` and click on `Next`
4. Give your dataset a name and configure the desired schema

Copy the URL of the dataset as we will be using it later on this notebook.

## Overview

On high level, the notebook does the following:

1. Creates a time series subscription
2. Creates a function that will perform the following actions:
    1. Fetch the latest cursor from the time series (if the time series doesn't exist, create it)
    2. Fetch the time series included in the subscription
    3. Fetch the metadata of the time series
    4. Fetch the updates from the subscription
    5. Check if there are any updates (if there are no updates, we can skip the rest of the logic)
    6. Create the payload to be pushed to Power BI (conforming to the schema of the dataset)
    7. Push the payload to Power BI (if the push succeeds, insert the new cursor in the time series)
3. Deploy the function to Cognite Functions
4. Creates a schedule for the function to run every 5 minutes