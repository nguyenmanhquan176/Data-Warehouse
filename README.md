# Project Title: 
Project: Data Warehouse

# Description: 
To build an ETL pipeline that extracts data from S3, stages them in Redshift, and transforms data into a set of dimensional tables for a analytics team.

# Structure
The project contains the following components:

* create_tables.py creates the Sparkify star schema in Redshift
* etl.py defines the ETL pipeline, extracting data from S3, loading into staging tables on Redshift, and then processing into analytics tables on Redshift
* sql_queries.py defines the SQL queries that underpin the creation of the star schema and ETL pipeline

# Datasets
Data is collected for song and user activities, in two directories: data/log_data and data/song_data, 2 files using JSON files.

Song dataset format
{
    "num_songs":1,
    "artist_id":"ARD7TVE1187B99BFB1",
    "artist_latitude":null,
    "artist_longitude":null,
    "artist_location":"California - LA",
    "artist_name":"Casual",
    "song_id":"SOMZWCG12A8C13C480",
    "title":"I Didn't Mean To",
    "duration":218.93179,
    "year":0
}

Log dataset format
{
    "artist":"Kenny G",
    "auth":"Logged In",
    "firstName":"Chloe",
    "gender":"F",
    "itemInSession":53,
    "lastName":"Cuevas",
    "length":256.57424,
    "level":"paid",
    "location":"San Francisco-Oakland-Hayward, CA",
    "method":"PUT",
    "page":"NextSong",
    "registration":1540940782796.0,
    "sessionId":648,
    "song":"Everlasting",
    "status":200,
    "ts":1542412944796,
    "userAgent":"Mozilla\/5.0 (Windows NT 5.1; rv:31.0) Gecko\/20100101 Firefox\/31.0",
    "userId":"49"
}

# Schema

## Staging events table

|   Column      |          Type         | Nullable | SORTKEY DISTKEY |
| --------------| -------------------   | ---------|---------------- |
| event_id      | BIGINT IDENTITY(0,1)  | not null |                 |
| artist        | VARCHAR               |          |                 |
| auth          | VARCHAR               |          |                 |
| firstName     | VARCHAR               |          |                 |
| gender        | VARCHAR               |          |                 |
| itemInSession | VARCHAR               |          |                 |
| length        | VARCHAR               |          |                 |
| level         | VARCHAR               |          |                 |
| location      | VARCHAR               |          |                 |
| method        | VARCHAR               |          |                 |
| page          | VARCHAR               |          |                 |
| registration  | VARCHAR               |          |                 |
| sessionId     | INTEGER               | not null |       YES       |
| song          | VARCHAR               |          |                 |
| status        | INTEGER               |          |                 |
| ts            | BIGINT                | not null |                 |
| userAgent     | VARCHAR               |          |                 |
| userId        | INTEGER               |          |                 |

## Staging songs table

|   Column        |          Type         | Nullable | SORTKEY DISTKEY |
| ----------------| -------------------   | ---------|---------------- |
| num_songs       | INTEGER               |          |                 |
| artist_id       | VARCHAR               | not null |       YES       |
| artist_latitude | VARCHAR               |          |                 |
| artist_longitude| VARCHAR               |          |                 |
| artist_location | VARCHAR(500)          |          |                 |
| artist_name     | VARCHAR(500)          |          |                 |
| song_id         | VARCHAR               | not null |                 |
| title           | VARCHAR(500)          |          |                 |
| duration        | DECIMAL(9)            |          |                 |
| year            | INTEGER               |          |                 |

## Songplays table
Records in log data associated with song plays i.e. records with page set to NextSong.

|   Column    |          Type         | Nullable | SORTKEY DISTKEY | DISTKEY |
| ----------- | --------------------- | ---------|---------------- |-------- |
| songplay_id | INTEGER IDENTITY(0,1) | not null |       YES       |         | 
| start_time  | TIMESTAMP             | not null |                 |         | 
| user_id     | VARCHAR(50)           | not null |                 |   YES   |
| level       | VARCHAR(10)           | not null |                 |         | 
| song_id     | VARCHAR(40)           | not null |                 |         | 
| artist_id   | VARCHAR(50)           | not null |                 |         | 
| session_id  | VARCHAR(50)           | not null |                 |         | 
| location    | VARCHAR(100)          |          |                 |         | 
| user_agent  | VARCHAR(255)          |          |                 |         | 

## Users table
Users table in the app.

|   Column   |       Type            | Nullable | SORTKEY DISTKEY | DISTKEY |
| ---------- | --------------------- | -------- | ----------------|---------|   
| user_id    | INTEGER               | not null |      YES        |         |
| first_name | VARCHAR(50)           |          |                 |         | 
| last_name  | VARCHAR(80)           |          |                 |         | 
| gender     | VARCHAR(10)           |          |                 |         | 
| level      | VARCHAR(10)           |          |                 |         | 

## Songs table
Songs table in music database.

|  Column   |         Type          | Nullable | SORTKEY DISTKEY | DISTKEY |
| --------- | --------------------- | -------- |---------------- | --------|      
| song_id   | VARCHAR(50)           | not null |      YES        |         |
| title     | VARCHAR(500)          | not null |                 |         |
| artist_id | VARCHAR(50 )          | not null |                 |         |
| year      | INTEGER               | not null |                 |         |
| duration  | DECIMAL(9)            | not null |                 |         |

## Artists table
Artists table in music database.

|  Column   |         Type          | Nullable | SORTKEY DISTKEY | DISTKEY |
| --------- | --------------------- | -------- |---------------- | --------|      
| artist_id | VARCHAR(50)           | not null |       YES       |         |
| name      | VARCHAR(500)          |          |                 |         |
| location  | VARCHAR(500)          |          |                 |         |
| latitude  | DECIMAL(9)            |          |                 |         |
| longitude | DECIMAL(9)            |          |                 |         |

## Time table
Timestamps of records in songplays broken down into specific units.

|   Column   |            Type             | Nullable | SORTKEY DISTKEY | DISTKEY |
| ---------- | --------------------------- | -------- |---------------- | --------|      
| start_time | TIMESTAMP                   | not null |       YES       |         |
| hour       | SMALLINT                    |          |                 |         |
| day        | SMALLINT                    |          |                 |         |
| week       | SMALLINT                    |          |                 |         |
| month      | SMALLINT                    |          |                 |         |
| year       | SMALLINT                    |          |                 |         |
| weekday    | SMALLINT                    |          |                 |         |

# Executing
From the Launcher -> Console click on Python 3. On the new tab names Console # run the command:

!python create_tables.py
!python etl.py

# Authors
Contributors names and contact info

ex. Nguyen Manh Quan
ex. @NguyenManhQuan