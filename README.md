# Integrating-Python-SQL-and-Tableau
Integrating Python, SQL, and Tableau

I used python to preprocess data related to employees at a company and applied machine learning (logistic regression) to predict which employees will be excessively absent.
I used the PyMySQL package to upload the final results to a MySQL table, and also visualised the predictions via Tableau. 

Below are the column names for the data I preprocessed;

ID, Reason for Absence, Date, Transportation Expense, Distance to Work, Age, Daily Work Load Average, Body Mass Index,	
Education, Children, Pets, Absenteeism Time in Hours

Please see my visualisations with below links
1. https://public.tableau.com/profile/rahul7180#!/vizhome/PSTDatavisulisation/ReasonsvsProbability
2. https://public.tableau.com/profile/rahul7180#!/vizhome/PSTDatavisulisation/AgevsProbability
3. https://public.tableau.com/profile/rahul7180#!/vizhome/PSTDatavisulisation/TransportationExpenseandChildren

Absenteeism Exercise - Preprocessing 2
---------------------------------------
This Jupyter notebook file showcases how I did the following

1. Used the Pandas package to import a CSV into Python
2. Assigned the data in the CSV to a python object (df)
3. Drop irrelevant columns (row ID Column dropped)
4. Create a python object to hold dummy values (reason_columns)
5. audit the new dummy value object to ensure the data was correct
6. Map(group) the values in the dummy frames to new data frames (reason_type_1, reason_type_2, reason_type_3, reason_type_4)
7. Drop the eason for Absence column from DF and concat reason_type_1, reason_type_2, reason_type_3, reason_type_4
8. rename columns in DF
9. Convert age column in DF to dummies (age_dummies)
10. create a new data frame (df_no_age) with the age column dropped
11. create a new data frame (df_concatenated) to hold the combination of df_no_age and age_dummies
12. Rename the columns in df_concatenated
13. Create data frame checkpoint (DF_reason_mod) of DF
14. Convert date column in df_reason_mod to datetime format with the format specified
15. use the .month method to extract the month from the date column in DF_reason_mod
16. use a for loop to add the month value to a Python List (list_months) for each row
17. append list_months to the data frame via a new column
18. use .weekday Pandas method to extract weekday from DF_reason_mod date column
19. created a function to return weekday (date_to_weekday) from a date value
20. used date_to_weekday to append the weekday to df_reason_mod via a new column 'day of the week'
21. Created a new data frame checkpoint (df_reason_mod2)
21. dropped the date column from df_reason_mod
22. renamed/reorganised df_reason_mod columns
23. Created a new checkpoint called df_reason_mod_date
24. used the .map pandas method to map the education column in df_reason_mod_date. Anyone below uni was mapped to 0 and anyone with higher education was mapped to 1
25. created a final checkpoint (df_preprocessed)
26. used the .to_csv pandas method to save df_preprocessed as a CSV

Absenteeism Exercise - Logistic Regression
------------------------------------------

1. Import pandas and numpy Python Packages
2. load the preprocessed CSV into a python object data_preprocessed
3. created a targets object which held the target data for the machine learning algorithm. The median of the 'Absenteeism Time in Hours' column in the data_preprocesed
   was used to determine the targets. Essentially any employee higher than the median was considered excessively absent
4. Added the new targets object to the data-preprocessed data frame
5. used the sum of the targets object and the total rows in the targets object to determine that 46% of employees were excessively absent
6. created a new data frame (data_with_targets) which was data_preprocessed with 'Absenteeism Time in Hours','Day of the Week','Daily Work Load Average', 'Distance to Work' columns dropped
7. created a new data frame (unscaled_inputs) and added all the columns in data_with targets except the last column
8. imported sklearn standardScaler
9. created a standardScaler object (absenteeism_scaler)
10.created a scaled_inputs object to hold the transformed unscaled_inputs. Used absenteeism_scaler to transfrom the unscaled_inputs
11. imported train_test_split from sklearn
12. Created 4 python objects to hold test and train data. Used the train_test_split package to split the data in scaled_inputs and targets as follows; 
    80% data will be used to train the model and 20% will be used to test the model. A random state of 20 was set to ensure data is randomised the same way to prevent model inaccuracy.
13. imported logistic regression model and metrics from sklearn
14. created a logistic regression object (reg)
15. Fitted reg with the training data
16. used the .score method to see model accuracy was 78%
17. then manually checked the model accuracy
18. Created a an object (model_outputs) to hold the predications of the model using training inputs
19. summed whenever the value in the model_outputs array = training targets
20. divided sum of step 19 by the total number of values in the training outputs. This showed model accuracy was 78%
21. tested the model with the test data and recieved 73% accuracy
22. imported pickle
23. created files with scaler and model

A python module can be created going forward to preproces the data and use the pickeled scaler and model

Absenteeism Exercise - Deploying the 'absenteeism_module'
-----------------------------------------------------------
1. Import the python module and create an object(model) that takes in the scaler and model files that were created
2. run the load_and_clean_data method agaisnt a new CSV with abseentism data to preprocess the csv
3. import pymysql
4. create a python object to connect to database
5. create a cursor object to interact with the database
6. create a new dataframe (df_new_obs) to hold the model dataframe with the predicted outputs
7. create an insert query object which holds 'INSERT INTO predicted_outputs VALUES'
8. use a nested for loop to update the inesert query with all the data in df_new_obs
9. clean up the insert query and add ';'
10. execute the insert query with the cursor object
11. commit the insert via the cursor
12. close the connection
 


