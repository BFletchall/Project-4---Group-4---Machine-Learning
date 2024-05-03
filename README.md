# Project-4-Group-4-Machine-Learning
<p align="center">
  <img width="600" height="400" src="https://www.pexels.com/photo/green-and-yellow-crane-439416/">
</p>


## Purpose

Wonder Woman Construction Company Inc currently bids for jobs based on an industry software program that calculates time via tasks. The task times used by the software are set based on historical company data, and WWC Inc. would like to build a tool that uses their current company task time benchmarks to estimate the bids. 

Team to utilize more recent historical data collected across multiple WWC jobs with actual time each task was completed. The tool build will include a model to analyze the actual task times and predict times for future jobs. WWC Inc could then estimate bids based on their current company data versus outdated historical data.  

 **The key questions this Analysis will answer are:**  
 - What tasks or jobs are under-estimated in terms of actual time versus bid time? 
- What tasks or jobs are overestimated in terms of actual time versus bid time? 
- How should WWC Inc. change their bid strategy based on this data? 
- Are there factors that impact timing that are not being considered via the estimate process? Can we find what data is used to put into the current estimate tool? 
- Is the custom model more accurate than software estimates? HME Inc thinks they are over-estimating time for jobs. Are they right? 

Below are the steps taken to setup and run the model:  



**A.Collection and Data Preparation.**

**B. Database setup and loading.**

**C. Data Preprocessing.**

**D. Feature Model Selection.**  

**E. Model Training.**  

**F. Model Evaluation.**

**G. Model Interpretation and Analysis.**



## 1. Collection and Data Preparation

This process involves the following items  
- Extract and transform the crowdfunding.xlsx Excel data to create a category DataFrame that has the following columns: 
    - Dropping unecessary columns not needed for the model  
    - Reordered columns so that the data is cohesive and makes sense
    - Grouped data to ensure analysis will be easy
    - Saved the cleaned data into a csv file "*combined_data.csv

    -   A 
    -   E

- E

    - A 

    - A 
        

## **2. Database setup and loading**  

This involves the following steps:  
- Extract and transform the crowdfunding.xlsx Excel data to create a campaign DataFrame has the following columns:

    - T

    - T

    - T

    - T

     

## **3. Data Preprocessing** 


Get dummies The team chose the following option for analysis and below are the steps taken for the analysis:  

- **Option 1:** Use Python dictionary methods  
    - Import the contacts.xlsx file into a DataFrame.   
    - Iterate through the DataFrame, converting each row to a dictionary. 
    - Iterate through each dictionary, doing the following:    
        - Extract the dictionary values from the keys by using a Python list comprehension.  
        - Add the values for each row to a new list.

    - Create a new DataFrame that contains the extracted data.  
    - Split each "name" column value into a first and last name, and place each in a new column.
    - Clean and export the DataFrame as contacts.csv and save it to your GitHub repository.  

## **4. Feature Model Selection**  

This involves the following steps:  
-   Inspect the four CSV files, and then sketch an ERD of the tables by using QuickDBDLinks to an external site..

-   Use the information from the ERD to create a table schema for each CSV file.
-   Save the database schema as a Postgres file named crowdfunding_db_schema.sql, and save it to your GitHub repository.

-   Create a new Postgres database, named crowdfunding_db.

-   Using the database schema, create the tables in the correct order to handle the foreign keys.

-   Verify the table creation by running a SELECT statement for each table.

-   Import each CSV file into its corresponding SQL table.

-   Verify that each table has the correct data by running a SELECT statement for each.

## **5. Model Training:**  
Model 
## **6. Model Evaluation:**  
Evaluation
## **7. Model Intepretation & Analysis:**  
Analysis 
## **8. Documents Created and Updated:**
 
- **data_prep.ipynb** [(Link)](https://github.com/manuel-sosa/Crowdfunding_ETL/blob/main/ETL_Mini_Project_Starter_Code-LJepkorir_MSosa_COMBINED.ipynb)  
    Contains the code of the initial ingestion of the data and initial data cleanup. 
- **SQLite_database_creation.ipynb**  
 Contains the code to setup the database and load data into the database  contains the following new documents [(Link)](https://github.com/manuel-sosa/Crowdfunding_ETL/tree/main/Resources):  
    
 - **data Folder** contains the following new documents [(Link)](https://github.com/manuel-sosa/Crowdfunding_ETL/tree/main/Resources):
    - **AppServices_AllClaimingWorkSegments.xlsx** = initial data collection file 
    - **combined_data.csv** = output of the initial data cleaning
    


- **models** contains the following new documents [(Link)](https://github.com/manuel-sosa/Crowdfunding_ETL/tree/main/Database): 
    - **claimed_time.db** = The database file 
    - **linear_regression_model.ipynb** = regression model file  
    - **random_forest_regression_model.ipynb** = First random forest regression optimization file
    - **random_forest_regression_model2.ipynb** = Second random forest regression optimization file 
- **analysis folder**  
 Contains the analysis of the model  optimization file
    