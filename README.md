## Week 3 Homework
<b><u>Important Note:</b></u> <p>You can load the data however you would like, but keep the files in .GZ Format. 
If you are using orchestration such as Airflow or Prefect do not load the data into Big Query using the orchestrator.</br> 
Stop with loading the files into a bucket. </br></br>
<u>NOTE:</u> You can use the CSV option for the GZ files when creating an External Table</br>

<b>SETUP:</b></br>
Create an external table using the fhv 2019 data. </br>
Create a table in BQ using the fhv 2019 data (do not partition or cluster this table). </br>
Data can be found here: https://github.com/DataTalksClub/nyc-tlc-data/releases/tag/fhv </p>

## Question 1:
What is the count for fhv vehicle records for year 2019?
- 65,623,481
- 43,244,696
- 22,978,333
- 13,942,414


BQ Query
SELECT count(dispatching_base_num) FROM '<dataset>.<external table>'

Q1 Answers
43244696



## Question 2:
Write a query to count the distinct number of affiliated_base_number for the entire dataset on both the tables.</br> 
What is the estimated amount of data that will be read when this query is executed on the External Table and the Table?

- 25.2 MB for the External Table and 100.87MB for the BQ Table
- 225.82 MB for the External Table and 47.60MB for the BQ Table
- 0 MB for the External Table and 0MB for the BQ Table
- 0 MB for the External Table and 317.94MB for the BQ Table 


BQ Shell Code
(code to get external table estimated mb)bq query --use_legacy_sql=false --dry_run "SELECT COUNT(DISTINCT affiliated_base_number) FROM '<dataset>.<external table>'
(code to get BQ table estimated mb)bq query --use_legacy_sql=false --dry_run "SELECT COUNT(DISTINCT affiliated_base_number) FROM '<dataset>.<native table>'


Q2 Answers
- 0 MB for the External Table and 317.94MB for the BQ Table 


## Question 3:
How many records have both a blank (null) PUlocationID and DOlocationID in the entire dataset?
- 717,748
- 1,215,687
- 5
- 20,332

BQ Query
SELECT (COUNTIF((PUlocationID IS NULL) and (DOlocationID IS NULL)))  AS num_null FROM `<dataset>.<external table>`;

Q3 Answers
-717748


## Question 4:
What is the best strategy to optimize the table if query always filter by pickup_datetime and order by affiliated_base_number?
- Cluster on pickup_datetime Cluster on affiliated_base_number
- Partition by pickup_datetime Cluster on affiliated_base_number
- Partition by pickup_datetime Partition by affiliated_base_number
- Partition by affiliated_base_number Cluster on pickup_datetime


Q4 Answers
- Partition by pickup_datetime Cluster on affiliated_base_number


## Question 5:
Implement the optimized solution you chose for question 4. Write a query to retrieve the distinct affiliated_base_number between pickup_datetime 03/01/2019 and 03/31/2019 (inclusive).</br> 
Use the BQ table you created earlier in your from clause and note the estimated bytes. Now change the table in the from clause to the partitioned table you created for question 4 and note the estimated bytes processed. What are these values? Choose the answer which most closely matches.
- 12.82 MB for non-partitioned table and 647.87 MB for the partitioned table
- 647.87 MB for non-partitioned table and 23.06 MB for the partitioned table
- 582.63 MB for non-partitioned table and 0 MB for the partitioned table
- 646.25 MB for non-partitioned table and 646.25 MB for the partitioned table



BQ Shell Code

(code to get BQ table estimated mb)
bq query --use_legacy_sql=false --dry_run "SELECT COUNT(DISTINCT affiliated_base_number) FROM '<dataset>.<native table>'

(code to get BQ table that is partitioned by pickup_datetime column and clustered by affiliated_base_number column estimated mb)
bq query --use_legacy_sql=false --dry_run "SELECT COUNT(DISTINCT affiliated_base_number) FROM '<dataset>.<native table_partitioned_clustered>'

Q5 Answers
- 647.87 MB for non-partitioned table and 23.06 MB for the partitioned table


## Question 6: 
Where is the data stored in the External Table you created?

- Big Query
- GCP Bucket
- Container Registry
- Big Table


Q6 Answers
-GCP bucket


## Question 7:
It is best practice in Big Query to always cluster your data:
- True
- False

Q7 Answers
-False. It is not useful if the data has too many granular detail.
