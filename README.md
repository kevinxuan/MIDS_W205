# Project 1: Query Project

- In the Query Project, you will get practice with SQL while learning about
  Google Cloud Platform (GCP) and BiqQuery. You'll answer business-driven
  questions using public datasets housed in GCP. To give you experience with
  different ways to use those datasets, you will use the web UI (BiqQuery) and
  the command-line tools, and work with them in Jupyter Notebooks.

#### Problem Statement

- You're a data scientist at Lyft Bay Wheels (https://www.lyft.com/bikes/bay-wheels), formerly known as Ford GoBike, the
  company running Bay Area Bikeshare. You are trying to increase ridership, and
  you want to offer deals through the mobile app to do so. 
  
- What deals do you offer though? Currently, your company has several options which can change over time.  Please visit the website to see the current offers and other marketing information. Frequent offers include: 
  * Single Ride 
  * Monthly Membership
  * Annual Membership
  * Bike Share for All
  * Access Pass
  * Corporate Membership
  * etc.

- Through this project, you will answer these questions: 

  * What are the 5 most popular trips that you would call "commuter trips"? 
  
  * What are your recommendations for offers (justify based on your findings)?

- Please note that there are no exact answers to the above questions, just like in the proverbial real world.  This is not a simple exercise where each question above will have a simple SQL query. It is an exercise in analytics over inexact and dirty data. 

- You won't find a column in a table labeled "commuter trip".  You will find you need to do quite a bit of data exploration using SQL queries to determine your own definition of a communter trip.  In data exploration process, you will find a lot of dirty data, that you will need to either clean or filter out. You will then write SQL queries to find the communter trips.

- Likewise to make your recommendations, you will need to do data exploration, cleaning or filtering dirty data, etc. to come up with the final queries that will give you the supporting data for your recommendations. You can make any recommendations regarding the offers, including, but not limited to: 
  * market offers differently to generate more revenue 
  * remove offers that are not working 
  * modify exising offers to generate more revenue
  * create new offers for hidden business opportunities you have found
  * etc. 

#### All Work MUST be done in the Google Cloud Platform (GCP) / The Majority of Work MUST be done using BigQuery SQL / Usage of Temporary Tables, Views, Pandas, Data Visualizations

A couple of the goals of w205 are for students to learn how to work in a cloud environment (such as GCP) and how to use SQL against a big data data platform (such as Google BigQuery).  In keeping with these goals, please do all of your work in GCP, and the majority of your analytics work using BigQuery SQL queries.

You can make intermediate temporary tables or views in your own dataset in BigQuery as you like.  Actually, this is a great way to work!  These make data exploration much easier.  It's much easier when you have made temporary tables or views with only clean data, filtered rows, filtered columns, new columns, summary data, etc.  If you use intermediate temporary tables or views, you should include the SQL used to create these, along with a brief note mentioning that you used the temporary table or view.

In the final Jupyter Notebook, the results of your BigQuery SQL will be read into Pandas, where you will use the skills you learned in the Python class to print formatted Pandas tables, simple data visualizations using Seaborn / Matplotlib, etc.  You can use Pandas for simple transformations, but please remember the bulk of work should be done using Google BigQuery SQL.

#### GitHub Procedures

In your Python class you used GitHub, with a single repo for all assignments, where you committed without doing a pull request.  In this class, we will try to mimic the real world more closely, so our procedures will be enhanced. 

Each project, including this one, will have it's own repo.

Important:  In w205, please never merge your assignment branch to the master branch. 

Using the git command line: clone down the repo, leave the master branch untouched, create an assignment branch, and move to that branch:
- Open a linux command line to your virtual machine and be sure you are logged in as jupyter.
- Create a ~/w205 directory if it does not already exist `mkdir ~/w205`
- Change directory into the ~/w205 directory `cd ~/w205`
- Clone down your repo `git clone <https url for your repo>`
- Change directory into the repo `cd <repo name>`
- Create an assignment branch `git branch assignment`
- Checkout the assignment branch `git checkout assignment`

The previous steps only need to be done once.  Once you your clone is on the assignment branch it will remain on that branch unless you checkout another branch.

The project workflow follows this pattern, which may be repeated as many times as needed.  In fact it's best to do this frequently as it saves your work into GitHub in case your virtual machine becomes corrupt:
- Make changes to existing files as needed.
- Add new files as needed
- Stage modified files `git add <filename>`
- Commit staged files `git commit -m "<meaningful comment about your changes>"`
- Push the commit on your assignment branch from your clone to GitHub `git push origin assignment`

Once you are done, go to the GitHub web interface and create a pull request comparing the assignment branch to the master branch.  Add your instructor, and only your instructor, as the reviewer.  The date and time stamp of the pull request is considered the submission time for late penalties. 

If you decide to make more changes after you have created a pull request, you can simply close the pull request (without merge!), make more changes, stage, commit, push, and create a final pull request when you are done.  Note that the last data and time stamp of the last pull request will be considered the submission time for late penalties.

Make sure you receive the emails related to your repository! Your project feedback will be given as comment on the pull request. When you receive the feedback, you can address problems or simply comment that you have read the feedback. 
AFTER receiving and answering the feedback, merge you PR to master. Your project only counts as complete once this is done.

---

## Parts 1, 2, 3

We have broken down this project into 3 parts, about 1 week's work each to help you stay on track.

**You will only turn in the project once at the end of part 3!**

- In Part 1, we will query using the Google BigQuery GUI interface in the cloud.

- In Part 2, we will query using the Linux command line from our virtual machine in the cloud.

- In Part 3, we will query from a Jupyter Notebook in our virtual machine in the cloud, save the results into Pandas, and present a report enhanced by Pandas output tables and simple data visualizations using Seaborn / Matplotlib.

---

## Part 1 - Querying Data with BigQuery

### SQL Tutorial

Please go through this SQL tutorial to help you learn the basics of SQL to help you complete this project.

SQL tutorial: https://www.w3schools.com/sql/default.asp

### Google Cloud Helpful Links

Read: https://cloud.google.com/docs/overview/

BigQuery: https://cloud.google.com/bigquery/

Public Datasets: Bring up your Google BigQuery console, open the menu for the public datasets, and navigate to the the dataset san_francisco.

- The Bay Bike Share has two datasets: a static one and a dynamic one.  The static one covers an historic period of about 3 years.  The dynamic one updates every 10 minutes or so.  THE STATIC ONE IS THE ONE WE WILL USE IN CLASS AND IN THE PROJECT. The reason is that is much easier to learn SQL against a static target instead of a moving target.

- (USE THESE TABLES!) The static tables we will be using in this class are in the dataset **san_francisco** :

  * bikeshare_stations

  * bikeshare_status

  * bikeshare_trips

- The dynamic tables are found in the dataset **san_francisco_bikeshare**

### Some initial queries

Paste your SQL query and answer the question in a sentence.  Be sure you properly format your queries and results using markdown. 

- What's the size of this dataset? (i.e., how many trips)  
  * Answer: There are 983,648 trips in total.
  * SQL query:  
  ```sql
  select count(distinct(trip_id))
  from `bigquery-public-data`.san_francisco.bikeshare_trips;
  ```

- What is the earliest start date and time and latest end date and time for a trip?
  * Answer: The earliest start date and time for a trip is 2013-08-29 09:08:00 UTC, and the latest end date and time for a trip is 2016-08-31 23:48:00 UTC.
  * SQL query:  
  ```sql
  select min(start_date), max(end_date) 
  from `bigquery-public-data`.san_francisco.bikeshare_trips;
  ```

- How many bikes are there?
  * Answer: There are 700 bikes.
  * SQL query:  
  ```sql
  select count(distinct(bike_number))
  from `bigquery-public-data`.san_francisco.bikeshare_trips;
  ```


### Questions of your own
- Make up 3 questions and answer them using the Bay Area Bike Share Trips Data.  These questions MUST be different than any of the questions and queries you ran above.

- Question 1: Among all trips, which station appears the most in start station and which station appears the most in end station? 
  * Answer: For both start station and end station, San Francisco Caltrain (Townsend at 4th) appears the most.
  * SQL query:  
  ```sql
  select start_station_name, count(bike_number)
  from `bigquery-public-data`.san_francisco.bikeshare_trips
  group by start_station_name
  order by count(bike_number) DESC
  limit 1;
  ```  
  ```select end_station_name, count(bike_number) from `bigquery-public-data`.san_francisco.bikeshare_trips group by end_station_name order by count(bike_number) DESC limit 1;```

- Question 2:  What's the longest duration for a trip (round to the closest number of days)?
  * Answer: The longest duration for a trip is 200 days. This seems to be an unreasonable value for a trip to last, so most likely there may be some errors in the trip recording system or that the bike has been stolen and not returned.
  * SQL query:  
  ```sql
  select round(max(duration_sec)/60/60/24)
  from `bigquery-public-data`.san_francisco.bikeshare_trips;
  ```

- Question 3: Which zip code appears the most among all trips with duration between 2000 and 3000 seconds? (excluding null)
  * Answer: Zip code 94107 appears the most among all trips with duration between 2000 and 3000 seconds.
  * SQL query:  
  ```sql
  select zip_code, count(bike_number)
  from `bigquery-public-data`.san_francisco.bikeshare_trips
  where duration_sec <=3000 and duration_sec >= 2000
  group by zip_code
  order by count(zip_code) DESC
  limit 2;
  ```

### Bonus activity queries (optional - not graded - just this section is optional, all other sections are required)

The bike share dynamic dataset offers multiple tables that can be joined to learn more interesting facts about the bike share business across all regions. These advanced queries are designed to challenge you to explore the other tables, using only the available metadata to create views that give you a broader understanding of the overall volumes across the regions(each region has multiple stations)

We can create a temporary table or view against the dynamic dataset to join to our static dataset.

Here is some SQL to pull the region_id and station_id from the dynamic dataset.  You can save the results of this query to a temporary table or view.  You can then join the static tables to this table or view to find the region:
```sql
#standardSQL
select distinct region_id, station_id
from `bigquery-public-data.san_francisco_bikeshare.bikeshare_station_info`
```

- Top 5 popular station pairs in each region

- Top 3 most popular regions(stations belong within 1 region)

- Total trips for each short station name in each region

- What are the top 10 used bikes in each of the top 3 region. these bikes could be in need of more frequent maintenance.

---

## Part 2 - Querying data from the BigQuery CLI 

- Use BQ from the Linux command line:

  * General query structure

    ```
    bq query --use_legacy_sql=false '
        SELECT count(*)
        FROM
           `bigquery-public-data.san_francisco.bikeshare_trips`'
    ```

### Queries

1. Rerun the first 3 queries from Part 1 using bq command line tool (Paste your bq
   queries and results here, using properly formatted markdown):

  * What's the size of this dataset? (i.e., how many trips)
    * Query:
    ```
      bq query --use_legacy_sql=false '
        select count(distinct(trip_id)) as total_trips
        from `bigquery-public-data`.san_francisco.bikeshare_trips'
    ```
    * Result:  
    ```
    +-------------+
    | total_trips |
    +-------------+
    |      983648 |
    +-------------+
    ```

  * What is the earliest start time and latest end time for a trip?
    * Query:
    ```
      bq query --use_legacy_sql=false '
        select min(start_date) as earliest_date, max(end_date) as latest_date
        from `bigquery-public-data`.san_francisco.bikeshare_trips'
    ```
    * Result:
    ```
    +---------------------+---------------------+
    |    earliest_date    |     latest_date     |
    +---------------------+---------------------+
    | 2013-08-29 09:08:00 | 2016-08-31 23:48:00 |
    +---------------------+---------------------+
    ```

  * How many bikes are there?
    * Query:
    ```
      bq query --use_legacy_sql=false '
        select count(distinct(bike_number)) as number_of_bikes
        from `bigquery-public-data`.san_francisco.bikeshare_trips'
    ```
    * Result:
    ```
    +-----------------+
    | number_of_bikes |
    +-----------------+
    |             700 |
    +-----------------+
    ```
2. New Query (Run using bq and paste your SQL query and answer the question in a sentence, using properly formatted markdown):

  * How many trips are in the morning vs in the afternoon?
    * Query:  
    ```
      bq query --use_legacy_sql=false '
        select count(trip_id) 
        from `bigquery-public-data`.san_francisco.bikeshare_trips
        where extract(hour from start_date) >=6 and extract(hour from end_date) <12 and duration_sec <= 6*60*60'
    ```  
    ```
    bq query --use_legacy_sql=false '
        select count(trip_id) 
        from `bigquery-public-data`.san_francisco.bikeshare_trips
        where extract(hour from start_date) >= 12 and extract(hour from end_date) < 18 and duration_sec <= 6*60*60'
    ```
    * Answer: There are 386,842 trips in the morning and 367,098 trips in the afternoon.


### Project Questions
Identify the main questions you'll need to answer to make recommendations (list
below, add as many questions as you need).

- Question 1: What's the average commute time for trips greater than 5 minutes and fewer than 2 hours? (Commute trips should be in this range)

- Question 2: What percentage of the same day trips are commute, i.e. start station not equal to end station? 

- Question 3: What are some stations with high availbilty of bikes and some stations with low availbility of bikes during midday of the weekdays?

- Question 4: What percentage of the people using bikes are customers? subscribers?

- Question 5: What's the average commute time for customers and subscribers respectively?

- Question 6: For subscribers, which start station and end station appear the most?

- Question 7: Which landmark appears the most among all the trips?

- Question 8: What's the average availibility by station?

- Question 9: What's the average straight line distance travelled for subscribers respectively?

- Question 10: What's the average straight line distance travelled for all subscribers from the popular stations?

- Question 11: For rides with start date and end date happening on the same day, Whch day of week has the most number of rides?

- Question 12: How many trips are in the range from 6 to 9am and from 5-8pm and that the start and end station are different?


### Answers

Answer at least 4 of the questions you identified above You can use either
BigQuery or the bq command line tool.  Paste your questions, queries and
answers below.

- Question 1: What percentage of the people using bikes are customers? subscribers?
  * Answer: 84% of the people using bikes are subscibers, and 16% of the people using bikes are customers. Hence if we want to perform marketing, we should target at subscribers. However, 16% is also not a small number, so it's better to create personalized offers for these two target groups. 
  * SQL query:
  ```SQL
  select bt.subscriber_type, round(count(bt.trip_id)/(select count(trip_id) as trip_count from `bigquery-public-data`.san_francisco.bikeshare_trips),2), 
    from `bigquery-public-data`.san_francisco.bikeshare_trips bt
    group by bt.subscriber_type;
```



- Question 2: Which landmark appears the most among all the trips? 
  * Answer: For both start and end station, San Francisco has the most number of trips occured, covering 90.6% of the total number of trips. This is reasonable, as San Francisco is more densely populated compared to other cities and that in densely populated cities riding bikes are more convenient than driving cars.
  * SQL query:
  ```SQL
      select bs.landmark as city, count(bt.trip_id) as count_trip
      from `bigquery-public-data`.san_francisco.bikeshare_trips bt inner join 
      `bigquery-public-data`.san_francisco.bikeshare_stations bs
      on bt.start_station_id = bs.station_id
      group by bs.landmark
      order by count_trip DESC;
  ```
  ```SQL
      select bs.landmark as city, count(bt.trip_id) as count_trip
      from `bigquery-public-data`.san_francisco.bikeshare_trips bt inner join 
      `bigquery-public-data`.san_francisco.bikeshare_stations bs
      on bt.end_station_id = bs.station_id
      group by bs.landmark
      order by count_trip DESC;
  ```


- Question 3: What percentage of the same day trips are commute, i.e. start station not equal to end station? 
  * Answer: 96.56% of the same day trips have end station different from start station. This even further shows that people use bikes as their commute transportation.
  * SQL query:
  ```sql
  select count(trip_id) /(select count(trip_id) as count_trips
    from `bigquery-public-data`.san_francisco.bikeshare_trips)
    from
    `bigquery-public-data`.san_francisco.bikeshare_trips
    where extract(day from start_date) = extract(day from end_date) and
    extract(month from start_date) = extract(month from end_date) and
    extract(year from start_date) = extract(year from end_date) and
    start_station_id != end_station_id;
    ```
  
- Question 4: What's the average and max straight line distance travelled for all riders? 
  * Answer: The average straight line distance for all riders is 1.53km. The maximum straight line distance is 76.18km.
  * SQL query:
  ```sql
  select avg(sqrt(pow((s_lat - e_lat)*110.574,2) + pow((s_lon - e_lon)*111.32,2))) as distance
    from
    (select bs.latitude as s_lat, bs.longitude as s_lon,
    bs2.latitude as e_lat, bs2.longitude as e_lon
    from `bigquery-public-data`.san_francisco.bikeshare_trips bt
    inner join `bigquery-public-data`.san_francisco.bikeshare_stations bs
    on bt.start_station_id = bs.station_id
    inner join `bigquery-public-data`.san_francisco.bikeshare_stations bs2
    on bt.end_station_id = bs2.station_id
    where extract(day from start_date) = extract(day from end_date) and
    extract(month from start_date) = extract(month from end_date) and
    extract(year from start_date) = extract(year from end_date) and
    start_station_id != end_station_id) d;
  ```
  


---

## Part 3 - Employ notebooks to synthesize query project results

### Get Going

Create a Jupyter Notebook against a Python 3 kernel named Project_1.ipynb in the assignment branch of your repo.

#### Run queries in the notebook 

At the end of this document is an example Jupyter Notebook you can take a look at and run.  

You can run queries using the "bang" command to shell out, such as this:

```
! bq query --use_legacy_sql=FALSE '<your-query-here>'
```

- NOTE: 
- Queries that return over 16K rows will not run this way, 
- Run groupbys etc in the bq web interface and save that as a table in BQ. 
- Max rows is defaulted to 100, use the command line parameter `--max_rows=1000000` to make it larger
- Query those tables the same way as in `example.ipynb`

Or you can use the magic commands, such as this:

```sql
%%bigquery my_panda_data_frame

select start_station_name, end_station_name
from `bigquery-public-data.san_francisco.bikeshare_trips`
where start_station_name <> end_station_name
limit 10
```

```python
my_panda_data_frame
```

#### Report in the form of the Jupter Notebook named Project_1.ipynb

- Using markdown cells, MUST definitively state and answer the two project questions:

  * What are the 5 most popular trips that you would call "commuter trips"? 
  
  * What are your recommendations for offers (justify based on your findings)?

- For any temporary tables (or views) that you created, include the SQL in markdown cells

- Use code cells for SQL you ran to load into Pandas, either using the !bq or the magic commands

- Use code cells to create Pandas formatted output tables (at least 3) to present or support your findings

- Use code cells to create simple data visualizations using Seaborn / Matplotlib (at least 2) to present or support your findings

### Resource: see example .ipynb file 

[Example Notebook](example.ipynb)

