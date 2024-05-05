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

- *1. Collection and Data Preparation.*

- *2. Database setup and loading.*

- *3. Data Preprocessing.*

- *4. Feature Model Selection & Model Training.*  

- *5. Model Evaluation.* 

- *6. Model Interpretation and Analysis*

## **1. Collection and Data Preparation**
We prepared the collected data by doing the following:   
- Removed unrelated columns 
- Removed data that was confidential e.g. employee and Employee ID information 
- Reordered columns so that the data is cohesive and makes sense
- Grouped data to ensure analysis will be easy
- Converted data into unified units e.g. some columns had time in hours and others had time in seconds. So changed the time to seconds so that its easy to compare the data 
- Saved the cleaned data into a csv file "*combined_data.csv
           

## **2. Database setup and loading**  
- Setup a SQLite database using sqlalchemy and stored the cleaned data file into the database to be utilized for running the model.

## **3. Data Preprocessing** 
Some of the things that we did to Preprocess the data so that its ready for training and modeling include:
- Handling missing values  
We removed rows of any values that were blank or did not provide any data values that were meaninful to the model
- Binning 
We reduced the complexity of the data by categorizing data based on Assembly main piece production code by having a cut off value and grouping values that were less than 50. This helped take care of any outlier jobs that occur occasionally and may have skewed the model
- Encoding Categorical Variables
We used the get dummies to ensure all the data was in trainable format to encode them.   
- Train test splitting was done to the data so that the model can have training and testing data  

## **4. Feature Model Selection & Model Training**  
- We used several models to determine the best model for the data i.e.  
    - Linear regression
    - Random Forest Regression Modeling
    - Random Forest Classifier

## **5. Model Evaluation:**  
We used several ways of evaluating the model for accuracy:  
- Random Forest Classifier accuracy 
- Random Forest Regression Score
- Random Forest Mean Absolute Error
- Confusion Matrix

## **6. Model Intepretation & Analysis:**  
The Analysis of the model 

## **8. Documents Created and Updated:**
 
- **data_prep.ipynb** [(Link)]  
    Contains the code of the initial ingestion of the data and initial data cleanup. 
- **SQLite_database_creation.ipynb**  
 Contains the code to setup the database and load data into the database  contains the following new documents [(Link)]  
    
 - **data Folder** contains the following new documents [(Link)]
    - **AppServices_AllClaimingWorkSegments.xlsx** = initial data collection file 
    - **combined_data.csv** = output of the initial data cleaning
    


- **models** contains the following new documents [(Link)]: 
    - **claimed_time.db** = The database file 
    - **linear_regression_model.ipynb** = regression model file  
    - **random_forest_regression_model.ipynb** = First random forest regression optimization file
    - **random_forest_regression_model2.ipynb** = Second random forest regression optimization file 
- **analysis folder**  
 Contains the analysis of the model  optimization file
    
