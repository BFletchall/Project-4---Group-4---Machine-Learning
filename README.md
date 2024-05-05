# Project-4-Group-4-Machine-Learning

<img src="https://github.com/BFletchall/Project-4-Group-4-Machine-Learning/blob/main/constructionimage.jpg" alt="GitHub Project Image" width="50%">


## Purpose

Wonder Woman Construction Company Inc currently bids for jobs based on an industry software program that calculates time via tasks. The task times used by the software are set based on historical company data, and WWC Inc. would like to build a tool that uses their current company task time benchmarks to estimate the bids. 

WWC has not attempted this task before because they did not have a robust data set of historical time spent per job. A new data collectiont tool was recently put in place at WWC, and this new data will be used to build a new model to estimate Job Times. If the new model build is successful, WWC could then estimate bids base on their current company data versus outdated historical data. 

 **The key questions this Analysis will answer are:**  
 - Is the new data collection tool able to provide enough historical data for a new model to be built?
 - What are the most important factors for estimating job time, and is the new data collection tool able to provide it?
 - Can a new model with a minimum of 80% accuracy be created from the current historical data? 
- How should WWC Inc. change their bid strategy based on this data? 

Below are the steps taken to setup and run the model.   

## **1. Collection and Data Preparation**
We prepared the collected data by revieweing all fields available via the new WWC data collection tool, and included only relevant fields. 

| Field                                   | Definition                                                                                | Relevant to Project? |
|-----------------------------------------|--------------------------------------------------------------------------------------------|----------------------|
| Assembly_MainPieceProductionCode        | Piece Category: Beam, Pipe, Door Plate, Backer Bar, Crane Grider, Bollard, Alum Pipe, Galv Stringer, etc. | Yes                  |
| Assembly_MainPartDimension             | Inches by pound                                                                           | Yes                  |
| Assembly_MainPartFinishDescr           | Abbreviation + finish of the part description                                             | Yes                  |
| Assembly_MainPartGrade                 | Grade of instance                                                                         | Yes                  |
| Assembly_MainPartLengthFt              | Part length                                                                               | Yes                  |
| Assembly_MainPartShape                 | Shape of instance                                                                         | Yes                  |
| Assembly_NumSmallParts                 | Number of things to assemble per instance (not per employee per instance)                 | Yes                  |
| Assembly_SurfaceAreaEachSqFt          | Part surface area                                                                         | Yes                  |
| Assembly_TotalQuantityInJob            | Quantity per instance                                                                     | Yes                  |
| Assembly_WeightEachLbs                | Part weight                                                                               | Yes                  |
| EPM_AdjustedStationName                | Use this column to identify which task of the instance it is on. One instance can contain multiple station names (or tasks). | Yes                  |
| EPM_ProductionControlItemID           | Unique identifier of a group of instances. Unique to job number and main mark.            | Yes                  |
| EPM_InstanceNumber                     | Instance Number - we want to find time spent per instance                                  | Yes                  |
| TimeInSeconds_ThisWorkSegment         | Time spent per instance in seconds - no need to calculate time from start and end, it done already | Yes, this is unit of time it takes per instance. TARGET OF MODEL |
| Assembly_EstTotalHours_ThisLaborGroup | Software Estimated time, in hours. This is what we are comparing to.                       | Yes, as this is current estimate of time being used by WWC to bid jobs, but needs to be converted to seconds. |
| Assembly_EstTotalSeconds_ThisLaborGroup | New column created that converted "Assembly_EstTotalHours_ThisLaborGroup" to seconds.       |                      |
| EPM_MainMark                            | Indentifer of group of ControlItemIds, which then have a group of instances                | Yes, but is a descriptor that is not necessary to model |
| NumberWithDash                         | Project number. Unique to site/project. Will contain multiple Main Marks that have mutiple ControlItemIds which then have mutiple instances | Yes, but is a descriptor that is not necessary to model |
| AdjustedEnd                            | Not in use, so remove                                                                     | No                   |
| AdjustedStart                          | Not in use, so remove                                                                     | No                   |
| AS_EntityID                            | Remove                                                                                     | No                   |
| AS_ProjectID                           | AppServices Project ID - used in the software. Unique to each project. But use Project Number instead, this is not needed. | No                   |
| Assembly_MainPartFinishAbbrev         | Finish of the part abbreviated - do not use                                                | No                   |
| EPM_LaborGroupID                       | Not needed                                                                                | No                   |
| EPM_LaborGroupName                     | Do not use for model, use later                                                            | No                   |
| EPM_LaborGroupNumber                   | Do not use for model, use later                                                            | No                   |
| EPM_LotNumber                          | Not relevant                                                                               | No                   |
| EPM_PhaseNumber                        | Not relevant                                                                               | No                   |
| EPM_ProductionControlID                | Software Name Production Control ID. Not needed                                             | No                   |
| EPM_SequenceID                         | Lot sequence - break job down into parts, not relevant                                      | No                   |
| EPM_StationID                          | Not needed                                                                                | No                   |
| EPM_StationName                        | Remove                                                                                     | No                   |
| EPMItemStationInstanceSummaryWorkSegmentID | Remove                                                                                     | No                   |
| JobName                                | Name of the site or project they are working on                                             | No                   |
| STS_Barcode                            | Not needed                                                                                | No                   |
| TimeClcok_WorkSegmentEnd               | Remove                                                                                     | No                   |
| TimeClock_WorkSegmentStart             | remove                                                                                     | No                   |
| TimeClockWorkSegmentRecordID           | Remove                                                                                     | No                   |
| TimeInSeconds_AdjustedTime            | Not in use, so remove                                                                     | No                   |
| TimeInSeconds_TimeClockWorkSegment    | Don’t need                                                                                | No                   |
| TC_EmployeeID                          | Time clock worker id                                                                      | No - no PII          |
| AS_EmployeeName                        | Name of the worker                                                                        | No - no PII          |
| TimeStamp_End                         | End of the instance work                                                                   | No, calc'd in other column |
| TimeStamp_Start                       | Start of the instance work                                                                 | No, calc'd in other column |

Run the "[data_prep.ipynb](https://github.com/BFletchall/Project-4-Group-4-Machine-Learning/blob/main/data_prep.ipynb)" to generate final cleaned data file.
           

## **2. Database setup and loading**  
- Setup a SQLite database using sqlalchemy and stored the cleaned data file into the database to be utilized for running the model.
- Run the "[SQLite_database_creation.ipynb](https://github.com/BFletchall/Project-4-Group-4-Machine-Learning/blob/main/SQLite_database_creation.ipynb)" to generate SQLite database.

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
    
