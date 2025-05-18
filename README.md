# Data Engineering Project

## Overview

This repository contains two main data engineering projects:
1.  A web scraping script (`Kavya.py`) designed to extract product information for moisturizers from Dermstore.com and store it in various systems (CSV, HDFS, MySQL).
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
    * 
---

## How to Run

* **Dermstore Web Scraper:**
    ```bash
    python Kavya.py
    ```
    *Note: This script performs extensive web scraping and database operations.*
