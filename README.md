# SpaceX Falcon 9 First Stage Landing Prediction
![](https://github.com/Penichezito/Falcon9-Launch-DataScience/blob/main/images/lifts-off.png)

## Introduction

This project aims to predict the success of SpaceX Falcon 9 first stage landings using machine learning. SpaceX achieves significant cost savings by reusing the first stage of its Falcon 9 rockets. Predicting landing success is crucial for determining launch costs and bidding against SpaceX for rocket launches.

## Objectives

The primary objective is to develop a machine learning model to predict the success of Falcon 9 first stage landings. This will involve exploring features from the collected data and training a suitable classification model.

## Steps

The notebook is organized into the following sections:

1. **Data Collection:** Utilizing the SpaceX API to collect relevant data on past Falcon 9 launches.
2. **Data Preprocessing:** Cleaning, wrangling, and transforming the collected data into a usable format.
3. **Feature Engineering:** Creating new features or manipulating existing features to improve model performance.
4. **Model Selection:** Selecting a suitable machine learning model for landing prediction.
5. **Model Training:** Training the selected model using the prepared dataset.
6. **Model Evaluation:** Evaluating the performance of the model using appropriate metrics.
7. **Prediction:** Using the trained model to predict the landing outcome of future Falcon 9 launches.

## Files

- `SpaceX_project.ipynb`: Jupyter Notebook containing the project code and analysis.
- `dataset_part_1.csv`: CSV file containing the cleaned and formatted launch data.
- `dataset_part_2.csv`: CSV file containing the final dataset with Data Wrangling and engineered features and target variable.
- `dataset_part_3.csv`: CSV file containing the final dataset with EDA.

## Usage Instructions

This notebook is designed to be run in a Google Colab environment or a similar Jupyter Notebook environment. To utilize this notebook effectively, follow these steps:

**1. Environment Setup:**

- Ensure you have a Google Colab account or a compatible Jupyter Notebook environment set up.
- Open the notebook in your preferred environment.

**2. Library Installation:**

- The notebook requires several Python libraries for data processing, visualization, and machine learning.
- Install the necessary libraries using `pip`:
Use code with caution
bash pip install requests pandas numpy datetime

 
**3. Data Loading:**

- The notebook utilizes a static JSON file for demonstration purposes. However, you can modify the code to fetch data directly from the SpaceX API if desired.
- Ensure that the JSON file or API access is properly configured within the notebook.

**4. Code Execution:**

- Execute the code cells in the notebook sequentially.
- Each cell performs a specific task, such as data loading, cleaning, transformation, analysis, or visualization.
- Carefully review the comments and documentation within the notebook to understand the purpose of each code block.

**5. Customization:**

- You can customize the notebook to explore different aspects of the data or to apply different machine learning techniques.
- Consider modifying the code to:
    - Extract additional features from the data.
    - Experiment with alternative data preprocessing methods.
    - Apply various machine learning models for prediction.
    - Evaluate model performance using different metrics.

**6. Output Interpretation:**

- Carefully interpret the outputs generated by the notebook, such as data visualizations, model performance metrics, and predictions.
- Use these insights to draw conclusions about the success of SpaceX Falcon 9 first stage landings.

**7. Further Exploration:**

- The notebook provides a foundation for further exploration and analysis.
- Consider extending the project by:
    - Investigating the impact of different features on landing success.
    - Developing more robust machine learning models.
    - Building interactive visualizations to present your findings.
    - Deploying the model for real-time predictions.

# **Step 1: Data Collection**

This project focuses on predicting the success of SpaceX Falcon 9 first stage landings using machine learning. A crucial initial step is the data collection process, which involves retrieving and organizing relevant data about past Falcon 9 launches.

### Data Source

The primary data source for this project is the SpaceX API. This API provides comprehensive information about various aspects of SpaceX missions, including launch details, rocket specifications, payload data, and more. For demonstration purposes, the notebook utilizes a static JSON file containing launch data, but the code can be easily modified to fetch data directly from the API.

### Data Extraction Process

The data collection stage involves a series of steps to retrieve and process the necessary information:

1. **API Interaction:** The code initiates by making requests to the SpaceX API using the `requests` library. It fetches data on past Falcon 9 launches.

2. **Helper Functions:** Four helper functions play a crucial role in extracting specific details from the API responses:
    - `getBoosterVersion`: Extracts the booster version (e.g., Falcon 9 Block 5) associated with a launch.
    - `getLaunchSite`: Retrieves information about the launch site, including its name, longitude, and latitude.
    - `getPayloadData`: Extracts data about the payload, such as its mass and the target orbit.
    - `getCoreData`: Gathers information about the rocket core, including landing outcomes, reuse status, and other relevant details.

3. **Data Parsing and Transformation:** The JSON responses from the API are parsed and converted into a Pandas DataFrame, providing a structured tabular representation of the data. This DataFrame undergoes further transformations, such as:
    - Filtering for Falcon 9 launches: The data is filtered to focus solely on Falcon 9 launches, excluding other rocket types like Falcon Heavy.
    - Feature selection: Relevant features for predicting landing success are selected from the DataFrame, including booster version, payload mass, launch site details, core landing outcomes, and more.
    - Data cleaning: The data is cleaned by handling missing values, converting data types (e.g., dates), and ensuring data consistency.

4. **Iterative Data Enrichment:** The helper functions are used iteratively to extract detailed information about each launch based on unique identifiers within the dataset. This process enriches the data with more specific details about the booster, launch site, payload, and core.

5. **DataFrame Construction:** Finally, the extracted data is organized into a dictionary and converted into a final Pandas DataFrame. This DataFrame contains a comprehensive collection of features relevant to predicting Falcon 9 first stage landing success.


### Data Attributes

The collected data includes various attributes relevant to the task, such as:

- `FlightNumber`: Unique identifier for each launch.
- `Date`: Date of the launch.
- `BoosterVersion`: Version of the Falcon 9 booster used.
- `PayloadMass`: Mass of the payload carried by the rocket.
- `Orbit`: Target orbit for the payload.
- `LaunchSite`: Location of the launch site.
- `Outcome`: Outcome of the core landing (success or failure).
- `Flights`: Number of flights the core has completed.
- `GridFins`: Whether grid fins were used during landing.
- `Reused`: Whether the core was previously reused.
- `Legs`: Whether landing legs were used.
- `LandingPad`: Identifier of the landing pad used.
- `Block`: Block version of the core.
- `ReusedCount`: Number of times the core has been reused.
- `Serial`: Serial number of the core.
- `Longitude`: Longitude of the launch site.
- `Latitude`: Latitude of the launch site.

### Data Storage

The final dataset is saved as a CSV file named `dataset_part_1.csv`. This file serves as the foundation for subsequent data wrangling, feature engineering, and model development stages.


# (Optional)**Data Collection and Processing using Web Scraping**

It involves making HTTP requests to the API endpoint and parsing the JSON response to extract relevant information.

## Project Overview

Retrieves SpaceX launch data using the SpaceX API, processes and cleans it, and exports it to a CSV file. The process includes data wrangling, extracting additional information through API calls, handling missing values, and filtering the data.

## Key Steps

1.  **Importing Libraries:**

    The following Python libraries are used:

    *   `requests`: For making HTTP requests to the SpaceX API.
    *   `pandas`: For data manipulation and analysis using DataFrames.
    *   `numpy`: For numerical operations.
    *   `datetime`: For working with dates and times.

    ```python
    import requests
    import pandas as pd
    import numpy as np
    import datetime
    ```

2.  **API Request:**

    *   The core URL for the SpaceX API is stored in the `spacex_url` variable.
    *   A GET request is made to the API using `requests.get(spacex_url)`.
    *   The response is converted to JSON format using `response.json()`.

    ```python
    spacex_url = "YOUR_SPACEX_API_URL" # Replace with the actual URL
    response = requests.get(spacex_url)
    data = response.json()
    ```

3.  **Data Wrangling:**

    *   The JSON data is normalized into a Pandas DataFrame named `data` using `pd.json_normalize()`.
    *   Relevant columns (`rocket`, `payloads`, `launchpad`, `cores`, `flight_number`, and `date_utc`) are selected.
    *   Data is filtered to keep only Falcon 9 launches and those with single cores and payloads.
    *   Nested data within `cores` and `payloads` is extracted into separate columns.
    *   The `date_utc` column is converted to a datetime object, and the date is extracted.
    *   Data is restricted to launches before November 13, 2020.

4.  **Extracting Additional Information:**

    *   Helper functions (`getBoosterVersion`, `getLaunchSite`, `getPayloadData`, and `getCoreData`) are defined to extract additional details from the API using specific IDs found in the initial data.
    *   These functions iterate through the data and make API requests for each launch to retrieve information about the booster version, launch site, payload mass, orbit, and core landing outcomes.

5.  **Building the Final Dataset:**

    *   The extracted data is stored in global lists (e.g., `BoosterVersion`, `PayloadMass`, `Orbit`, etc.).
    *   These lists are combined into a dictionary `launch_dict`.
    *   The dictionary is converted into a Pandas DataFrame named `launch_df`.
    *   The DataFrame is filtered to keep only Falcon 9 launches.
    *   Flight numbers are reset to sequential order.

6.  **Handling Missing Values:**

    *   Missing values in the `PayloadMass` column are replaced with the mean value using the `replace()` function.

7.  **Data Export:**

    *   The cleaned dataset is exported to a CSV file named `dataset_part_1.csv` using `data_falcon9.to_csv()`.

    ```python
    data_falcon9.to_csv('dataset_part_1.csv', index=False)
    ```

## File Output

The project outputs a CSV file named `dataset_part_1.csv` containing the processed SpaceX launch data.

# **Step 2: Data Preprocessing - Data Wrangling**

This step focuses on understanding the data and preparing it for machine learning. We perform the following tasks:

**1. Exploratory Data Analysis (EDA):**

   - **Data Understanding:** We use various techniques like descriptive statistics (mean, median, standard deviation), data visualization (histograms, scatter plots), and data profiling to gain insights into the dataset. This helps us understand the distribution of data, identify potential outliers, and discover relationships between features.
   - **Missing Value Analysis:** We identify and handle missing values in the dataset. Missing values can negatively impact model performance, so we either impute them (replace with estimated values) or remove the corresponding data points.
   - **Data Cleaning:** We perform data cleaning operations such as removing duplicates, correcting inconsistencies, and converting data types to ensure data quality.

**2. Determining Training Labels:**

   - **Target Variable:** We identify the target variable, which is the outcome we want to predict (in this case, whether the first stage of the Falcon 9 rocket landed successfully).
   - **Label Encoding:** We convert the target variable into a binary format (0 for unsuccessful landing, 1 for successful landing) suitable for supervised learning models. This involves analyzing the different landing outcomes and categorizing them as either successful or unsuccessful.

**3. Feature Analysis and Relationship with Landing Outcomes:**

   - **Feature Importance:** We analyze the relationship between different features (e.g., payload mass, orbit, launch site) and the landing outcome. This helps us understand which features are most influential in predicting landing success.
   - **Correlation Analysis:** We investigate the correlation between features to identify potential multicollinearity (high correlation between independent variables). This is important to avoid redundancy and improve model interpretability.

**4. Feature Engineering:**

   - **Creating New Features:** Based on our understanding of the data and the relationships between features, we might engineer new features that can enhance model performance. This could involve combining existing features, creating interaction terms, or transforming variables.
   - **Feature Scaling:** We might scale or standardize features to ensure they have a similar range of values. This can help improve the performance of certain machine learning algorithms.
   - 

**5. Success Rate Calculation:**

   - **Overall Success Rate:** We calculate the overall success rate of launches by dividing the number of successful landings by the total number of launches. This gives us a baseline measure of launch success.
   - **Success Rate by Feature:** We might also calculate the success rate based on different feature values (e.g., success rate for launches from a particular launch site) to gain further insights.

# Step 3: Exploratory Data Analysis (EDA)

This step focuses on performing Exploratory Data Analysis (EDA) to gain insights into the SpaceX launch data and identify patterns that influence launch success.

## Objectives

*   **Exploratory Data Analysis:** Analyze the data to understand relationships between variables and their impact on launch outcomes.
*   **Feature Selection:** Identify key features that contribute significantly to predicting launch success.

## Data Loading

The dataset used in this step is loaded from the CSV file "dataset\_part\_2.csv," which was created in the previous step.

## Data Visualization

### Flight Number vs. Payload Mass

*   A scatter plot is generated to visualize the relationship between Flight Number and Payload Mass, with the launch outcome (Class) represented by color.
*   **Observation:** As the flight number increases, the likelihood of successful first-stage landing also increases. Payload mass appears to have a less significant impact, with successful landings occurring even with heavier payloads.

### Flight Number vs. Launch Site

*   A categorical plot is created to show the relationship between Flight Number and Launch Site, with the launch outcome (Class) indicated by color.
*   **Observation:** The success rate of landings tends to increase linearly with the number of attempts. Different launch sites exhibit varying patterns of success over time.

### Payload Mass vs. Launch Site

*   A categorical plot is generated to visualize the relationship between Payload Mass and Launch Site, with the launch outcome (Class) represented by color.
*   **Observation:** The VAFB-SLC launch site does not handle rockets with heavy payload mass (greater than 10,000 kg).

### Success Rate of Each Orbit Type

*   A bar chart is created to display the success rate of each orbit type.
*   **Observation:** Orbits like ESL-11, GEO, HEO, and SSO demonstrate higher success rates compared to others.

### Flight Number vs. Orbit Type

*   A scatter plot is generated to visualize the relationship between Flight Number and Orbit type, with the launch outcome (Class) indicated by color.
*   **Observation:** This plot helps identify trends in launch success for different orbit types over time.

### Payload Mass vs. Orbit Type

*   A scatter plot is created to visualize the relationship between Payload Mass and Orbit type, with the launch outcome (Class) represented by color.
*   **Observation:** Polar, LEO, and ISS orbits tend to have more successful landings with heavy payloads. GTO orbits show mixed results, making it difficult to discern a clear pattern.

### Launch Success Yearly Trend

*   A line chart is generated to illustrate the average launch success rate over the years.
*   **Observation:** The success rate shows an increasing trend from 2013 to 2020.

## Feature Engineering

### Feature Selection

*   Based on the insights gained from EDA, the following features are selected for use in the subsequent prediction model:
    *   Flight Number
    *   Payload Mass
    *   Orbit
    *   Launch Site
    *   Flights
    *   Grid Fins
    *   Reused
    *   Legs
    *   Landing Pad
    *   Block
    *   Reused Count
    *   Serial

### Dummy Variables

*   One-hot encoding is applied to categorical features (Orbit, Launch Site, Landing Pad, and Serial) to create dummy variables. This transforms categorical data into numerical representations suitable for machine learning models.

### Data Type Conversion

*   All numeric columns in the dataset are cast to the data type 'float64' for consistency and compatibility with machine learning algorithms.

### Data Export

*   The final processed dataset, including the selected features and dummy variables, is exported to a CSV file named ´dataset\_part\_3.csv´ for use in the next steps of the project.


## (Step 3 Optional)**EDA with SQL - Exploratory Data Analysis**

**Key Features:**

* **Data Loading and Preprocessing:** The notebook demonstrates how to download the SpaceX dataset as a CSV file and load it into a SQLite database using pandas.
  
* **SQL Queries:** The notebook includes a series of SQL queries to answer various questions about the dataset, such as:
    * Identifying unique launch sites
    * Calculating total payload mass for specific customers
    * Determining average payload mass for booster versions
    * Finding the date of the first successful ground pad landing
    * Listing boosters with successful drone ship landings and specific payload mass ranges
    * Analyzing mission outcomes
    * Identifying boosters with maximum payload mass
    * Extracting launch data for specific months and years
    * Ranking landing outcomes within a date range
* **Data Visualization:** While the current version primarily focuses on SQL queries, you can extend it to include data visualizations to further enhance the analysis.

## Usage

1. **Clone the repository:**
2. **Open the Jupyter Notebook:**
   Navigate to the cloned repository and open the `SQL_Notebook_Falcon_9_SpaceX_dataset.ipynb` file in Jupyter Notebook or Google Colab.

3. **Run the notebook cells:**
   Execute the notebook cells sequentially to load the data, execute the SQL queries, and view the results.

4. **Explore the analysis:**
   Examine the results of the SQL queries to gain insights into the SpaceX Falcon 9 launch data.

## Requirements

* **Python 3:** Ensure you have Python 3 installed on your system.
* **Libraries:** The notebook requires the following libraries:
    * pandas
    * sqlalchemy
    * ipython-sql
    * sqlite3
    * prettytable
 
