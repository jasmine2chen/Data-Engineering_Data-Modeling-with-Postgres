# Data Engineering: Data Modeling with PostgreSQL

## Background
A startup called Sparkify wants to analyze the data they've been collecting on songs and user activity on their new music streaming app. The analytics team is particularly interested in understanding what songs users are listening to. Currently, they are looking for an easy way to query their data, which resides in a directory of JSON logs on user activity on the app, as well as a directory with JSON metadata on the songs in their app.

## Project Description
As a data engineer, I will conduct data modeling with Postgres and build an ETL pipeline that transfers data from files in two local directories into these tables in Postgres using Python and SQL. 

## Python scripts

- create_tables.py: Create the spartifydb database. Drop If it already exists. 
- sql_queries.py: CREATE songplay, user, song, artist, time TABLES. Drop If it already exists. All queries used in the ETL pipeline.
- etl.py: Read JSON log data and JSON song data, transform the datasets and upload the data into generated tables.

## Database Schema
### Dimension Tables
- **users**  (users in the app): 
user_id, first_name, last_name, gender, level
- **songs**  (songs in music database): 
song_id, title, artist_id, year, duration
- **artists**  (artists in music database): 
artist_id, name, location, latitude, longitude
- **time** (timestamps of records in songplays broken down into specific units): 
start_time, hour, day, week, month, year, weekday

### Fact Table
- **songplays** (records in log data associated with song plays):  
songplay_id, start_time, user_id, level, song_id, artist_id, session_id, location, user_agent




## ETL Pipeline Results


### song/artist ETL from song JSON data 

Select target columns; Use df.values to select just the values from the dataframe; Convert the array to a list and set it to data; Implement the table_insert query in sql_queries.py and insert the song/artist record into the song/artist table

#### Final tabes
- songs table

| song_id            | title                          | artist_id          | year | duration  |
|--------------------|--------------------------------|--------------------|------|-----------|
| SOFNOQK12AB01840FC | Kutt Free (DJ Volume Remix)    | ARNNKDK1187B98BBD5 | -    | 407.37914 |
| SOFFKZS12AB017F194 | A Higher Place (Album Version) | ARBEBBY1187B9B43DB | 1994 | 236.17261 |

- artist table

| artist_id          | name      | location        | lattitude | longitude |
|--------------------|-----------|-----------------|-----------|-----------|
| ARNNKDK1187B98BBD5 | Jinx      | Zagreb Croatia  | 45.80726  | 15.9676   |
| ARBEBBY1187B9B43DB | Tom Petty | Gainesville, FL | -         | -         |


### time/user ETL from log JSON data
Filter records by NextSong action; Convert the ts timestamp column to datetime; Extract the timestamp, hour, day, week of year, month, year, and weekday from the ts column and set time_data to a list containing these values in order; Specify labels for these columns and set to column_labels; Create a dataframe, time_df, containing the time data for this file by combining column_labels and time_data into a dictionary and converting this into a dataframe

#### Final tabes

- time table

| start_time                 | hour | day | week | month | year | weekday |
|----------------------------|------|-----|------|-------|------|---------|
| 2018-11-29 00:00:57.796000 | 0    | 29  | 48   | 11    | 2018 | 3       |
| 2018-11-29 00:01:30.796000 | 0    | 29  | 48   | 11    | 2018 | 3       |


- users table

| user_id | first_name | last_name | gender | level |
|---------|------------|-----------|--------|-------|
| 79      | James      | Martin    | M      | free  |
| 52      | Theodore   | Smith     | M      | free  |

### songplay ETL 
Implement the `song_select` query in `sql_queries.py` to find the song ID and artist ID based on the title, artist name, and duration of a song; Select the timestamp, user ID, level, song ID, artist ID, session ID, location, and user agent and set to `songplay_data`; Implement the `songplay_table_insert` query and run the cell below to insert records for the songplay actions in this log file into the `songplays` table.

- songplays table

| songplay_id | start_time                 | user_id | level | song_id | artist_id | session_id | location                            | user_agent                                                                                                              |
|-------------|----------------------------|---------|-------|---------|-----------|------------|-------------------------------------|-------------------------------------------------------------------------------------------------------------------------|
| 1           | 2018-11-29 00:00:57.796000 | 73      | paid  | -       | -         | 954        | Tampa-St. Petersburg-Clearwater, FL | "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_9_4) AppleWebKit/537.78.2 (KHTML, like Gecko) Version/7.0.6 Safari/537.78.2" |
| 2           | 2018-11-29 00:01:30.796000 | 24      | paid  | -       | -         | 984        | Lake Havasu City-Kingman, AZ        | "Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/36.0.1985.125 Safari/537.36"         |

