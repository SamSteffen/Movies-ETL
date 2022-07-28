# Movies-ETL
The following analysis presents documentation of an Extract-Transform-Load procedure performed on movie data using python's pandas dependencies, Jupyter notebooks, and SQL. To help a client create a pipeline for raw data pulled from a variety of internet sources, clean it, merge it and store it in a PgAdmin database, the following steps were taken.

### Extracting the Data
- Movie data was extracted from Wikipedia into a JSON file (stored as "wikipedia-movies.json"). Initially, this file contained 7,311 records from movies released over the past 30 years.
- Additional movie data was extracted from kaggle.com (stored as "kaggle_metadata.csv"). Initially, this file contained 45,466 rows of data from movies released over the past 30 years. 
- Still more movie data was extracted from MovieLens (stored as "ratings.csv"). This file initially contained 26,024,289 rows of data.

### Transforming the Data
Using python and Jupyter notebooks, the above data was inspected and cleaned.  A variety of operations were performed to make sure that the data being utilized in the final dataset was relevant and useful for the Hackathon database requested by the client. Among the many procedures performed were the following: 
* To clean the Wikipedia dataset: 
    - Used list comprehension to filter out data in columns unrelated to movies.
    - Created a function clean_movie() to clean the data by changing column names.
    - Developed a code that isolated a unique movie ID from a web url.
    - Converted and parsed data using regular expression matches to homogenize data types.

* To clean the Kaggle meta_data dataset:
    - Identified 6 columns that needed to be converted into a different data type from what was originally provided and converted them.

* To clean the MovieLens rating dataset:
    - The ratings info was consolidated by converting the data types in the "timestamp" column to a timestamp data type.

### Loading the Data
Once the data from the 3 sources was cleaned, it was merged into a dataframe and imported into a table in postgres, where it can be easily queried using SQL.

### Further Analysis
Following this initial Extract-Transform-Load procedure on the movie data analysis, we were asked to refactor our code to make the procedure easier for future users as well as potentially compatible for other data pipeline projects. A full description of this refactoring process and its outcomes is below.


# Results
To create an automated pipeline that takes in new data, performs the appropriate transformations, and loads the data into existing tables, the code from the initial analysis was refactored to create a single function that takes in the three files—Wikipedia data, Kaggle metadata, and the MovieLens rating data—and performs the ETL process by adding the data to a PostgreSQL database. This was accomplished in the following 4 segments:

### Step 1:
Using Python, Pandas, the ETL process, and code refactoring, we composed a function that reads in the three data files and creates three separate DataFrames.

### Step 2:
In the second portion of the analysis, we used our previously developed code to create a function that would extract, transform and load the data from the Wikipedia JSON file into a pandas dataframe. This was done to prepare it to be merged with the Kaggle metadata. While extracting the IMBDb IDs, we also employed a try-except block to catch errors.

### Step 3:
In the third portion of the analysis, we further refactored our code from the first and second deliverables by adding additional code that would, in addition to performing an ETL process on the Wikipedia JSON file, also perform a seperate ETL function on the kaggle data ("movies_metadata.csv") and the MovieLens rating data ("ratings.csv"). Each of these was converted into seperate dataframes in preparation for loading them into a pgAdmin database.

### Step 4:
In the final step we added more code to the function above to export the dataframes created in the third deliverable to Postgres as a database containing two tables, "movies" and "ratings". 

# Summary
While the python code we developed successfully managed to export and transform the data from the datasources and then upload it to a SQL database, the final output of Step 4 returned a TypeError that read "NoneType object is not iterable." While many procedures were done to troubleshoot this, one of the drawbacks of refactoring large blocks of code that are performed within functions is that it makes it difficult to isolate errors of the sort we received. Ultimately, the code ran successfully and imported the 26,024,289 rows of data from the ratings table in 2,287 seconds, a little over 38 minutes. Because of this, it is our recommendation that if additional cleaning of the data is desired, it be done using SQL querying upon the newly loaded tables. 

Additionally, it was the estimate that following execution of our ETL code, it would return 6,052 rows for the movies data. The code used in this ETL process generated a return of 6,091 rows, indicating that some of the desired data was not dropped or extracted as requested. While the tables can always be dropped and re-uploaded, for the sake of time, we recommend undertaking further inspection of the data using SQL query methods upon the uploaded dataframes.
