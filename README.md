# Data Engineering Project

## Overview

This repository contains two main data engineering projects:
1.  A web scraping script (`Kavya.py`) designed to extract product information for moisturizers from Dermstore.com and store it in various systems (CSV, HDFS, MySQL).
2.  A data science notebook (`Project_Work.ipynb`) focused on predicting house prices in King County, USA, using machine learning techniques.

---

## Components

### 1. Dermstore Web Scraper (`Kavya.py`)

* **Purpose:** To automate the collection of data for moisturizer products listed on the Dermstore website (dermstore.com).
* **Functionality:**
    * Uses Selenium with ChromeDriver to navigate the Dermstore moisturizer category pages, handling dynamic content and pagination.
    * Scrapes detailed product information including:
        * Product Name
        * Price
        * Size/Volume
        * Rating (out of 5)
        * Number of Reviews
        * Key Ingredients
        * Image URL
        * Targeted Skin Concerns
        * Suitable Skin Types
    * Downloads product images using the `requests` library and saves them locally in a `product_images` folder.
    * Accepts cookie consent pop-ups if present.
    * Implements scraping politeness features like random delays and user-agent rotation.
    * Collects up to 1000 product entries.
    * Saves the collected data into a CSV file named `dermstore_products.csv`.
    * Includes functions to upload the generated CSV file to HDFS (using `hdfscli` with a pre-configured alias 'dev').
    * Includes functions to insert the scraped product data into a MySQL database, mapping the data to tables like `Brand`, `Product`, `Image`, `SkinConcern`, `SkinType`, `Ingredient`, etc.
* **Dependencies:**
    * Python 3
    * External Libraries: `requests`, `pandas`, `selenium`, `webdriver-manager`, `pymysql`, `hdfs`
    * Standard Libraries: `os`, `subprocess`, `time`, `random`, `re`, `csv`
* **Usage:**
    1.  Ensure all dependencies are installed (`pip install requests pandas selenium webdriver-manager pymysql hdfs`).
    2.  Make sure ChromeDriver is installed or can be managed by `webdriver-manager`.
    3.  Configure HDFS CLI with an alias named 'dev' if you intend to use the HDFS upload feature.
    4.  Update the MySQL connection details (host, user, password, database name) within the script if using the database insertion feature. Ensure the target database schema exists.
    5.  Run the script from the command line: `python Kavya.py`
* **Output:**
    * `dermstore_products.csv`: Contains the scraped product data.
    * `product_images/`: A folder containing downloaded product images.
    * Data uploaded to `/data-engineering/dermstore_products.csv` on the HDFS cluster associated with the 'dev' alias.
    * Data inserted into the configured MySQL database tables.

### 2. King County House Price Prediction (`Project_Work.ipynb`)

* **Purpose:** To build and evaluate machine learning models for predicting house prices based on the King County House Sales dataset.
* **Methodology:**
    * **Data Loading:** Loads the dataset from `kc_house_data.csv`.
    * **Exploratory Data Analysis (EDA):** Visualizes data distributions (histograms, box plots) and relationships (scatter plots, pair plot, correlation heatmap).
    * **Preprocessing:**
        * Checks for missing values.
        * Handles outliers in the 'price' column using the Interquartile Range (IQR) method.
        * Applies log transformation to the 'price' feature to handle skewness.
        * Uses StandardScaler to scale numerical features for models sensitive to feature scales (like Linear Regression, XGBoost).
    * **Feature Engineering:** Creates a `price_per_sqft` feature.
    * **Model Training:** Trains and evaluates the following regression models:
        * Linear Regression (as a baseline)
        * Random Forest Regressor
        * XGBoost Regressor
    * **Ensemble Method:** Implements a custom weighted average ensemble of the Linear Regression, Random Forest, and XGBoost models.
    * **Evaluation:** Uses Mean Squared Error (MSE) and R-squared (R2) metrics. Visualizes predictions vs. actual values.
    * **Feature Importance:** Identifies and plots the top 4 most important features using the Random Forest model.
* **Dataset:**
    * King County House Sales dataset.
    * Source: [https://www.kaggle.com/datasets/harlfoxem/housesalesprediction?resource=download](https://www.kaggle.com/datasets/harlfoxem/housesalesprediction?resource=download)
    * The notebook expects the file `kc_house_data.csv` to be present in the same directory or uploaded during the session (as indicated by the `google.colab.files.upload()` call).
* **Dependencies:**
    * Python 3
    * Libraries: `pandas`, `numpy`, `scikit-learn`, `matplotlib`, `seaborn`, `xgboost`, `scipy`
    * Environment: Jupyter Notebook or Google Colab.
* **Usage:**
    1.  Ensure all dependencies are installed (`pip install pandas numpy scikit-learn matplotlib seaborn xgboost scipy`).
    2.  Download the `kc_house_data.csv` file from the Kaggle link provided.
    3.  Place the CSV file in the same directory as the notebook or be prepared to upload it when prompted (if running in Colab).
    4.  Open and run the `Project_Work.ipynb` notebook cell by cell in a compatible environment (Jupyter, Colab, etc.).
* **Output:**
    * Visualizations (histograms, scatter plots, heatmap, feature importance plot).
    * Model performance metrics (MSE, R2) printed for each model and the ensemble.
    * Prediction vs. Actual plot for the XGBoost model.

---

## Setup & Installation

1.  **Clone the repository:**
    ```bash
    git clone <repository_url>
    cd <repository_directory>
    ```
2.  **Install Python dependencies:**
    ```bash
    pip install requests pandas selenium webdriver-manager pymysql hdfs numpy scikit-learn matplotlib seaborn xgboost scipy notebook
    ```
3.  **Web Scraper Specific Setup:**
    * Ensure you have Google Chrome installed for Selenium.
    * (Optional) Configure `hdfscli` with the 'dev' alias pointing to your HDFS cluster.
    * (Optional) Set up a MySQL database with the required schema and update connection details in `Kavya.py`.
4.  **Prediction Notebook Specific Setup:**
    * Download `kc_house_data.csv` from Kaggle.

---

## How to Run

* **Dermstore Web Scraper:**
    ```bash
    python Kavya.py
    ```
    *Note: This script performs extensive web scraping and database operations.*

* **House Price Prediction:**
    1.  Start Jupyter Notebook:
        ```bash
        jupyter notebook
        ```
    2.  Navigate to and open `Project_Work.ipynb`.
    3.  Run the cells sequentially. Ensure `kc_house_data.csv` is accessible.
    *(Alternatively, upload and run in Google Colab)*
