### MY CREDENTIALS!

Load the file ".crendentials.R" with my credentials (find on computer or in personal notes).

______________

**3. From R, do the following in your Postgres server (i.e. the Postgres
server running in your postgres container)**

-  In the ”intg1” schema, create a table called ”students” with the following
columns:
    - Student_id: an autoincrementing integer column holding the primary key
   - Student_name: A varchar column where each entry can hold a maximum of 255
  characters
   - department_code: An integer column
- Insert two students into the ”students” table using the ” psql_manipulate()”
function.
- Insert two students into the ”students” table using the ”psql_append_df()”
function
- Use the ”psql_select()” function to fetch the ”students” data from Postgres
and confirm that your inserts were successful
```
library(RPostgres)
library(DBI)


# Put the credentials in this script
# Never push credentials to git!! --> use .gitignore on .credentials.R
source(".credentials.R")

# Function to send queries to Postgres
source("psql_queries.R")

# Create a new schema in Postgres on docker
psql_manipulate(cred = cred_psql_docker, 
                query_string = "CREATE SCHEMA intg1;") # schema called intg1

# Create a table in the new schema 
psql_manipulate(cred = cred_psql_docker, 
                query_string = 
"create table intg1.Students (
	student_id serial primary key,
	student_name varchar(255),
	department_code int,
	last_update timestamp(0) without time zone default current_timestamp(0)
);")

# Write rows in the new table
psql_manipulate(cred = cred_psql_docker, 
                query_string = 
"insert into intg1.Students
	values (default, 'Peter', 2)
		  ,(default, 'Klem', 3);")

# Create an R dataframe
df <- data.frame(student_name = c("Christian", "Jeff"),
                 department_code = c(1, 3))
# Write the dataframe to a postgres table (columns with default values are skipped)

department <- psql_append_df(cred = cred_psql_docker, 
                             schema_name = "intg1", 
                             tab_name = "students", # no capital letters! 
                             df = df)
# Fetching rows into R
psql_select(cred = cred_psql_docker, 
            query_string = "select * from intg1.Students;")
```
If schema should be deleted
```
# Delete schema
psql_manipulate(cred = cred_psql_docker, 
                query_string = "drop SCHEMA intg1 cascade;")
```

**5. Using the ”Alpha Vantage” API: Search for another stock (preferably given in "UTC-04” timezone – which corresponds to the "US/Eastern” timezone), find its symbol, and fetch its intraday price data with an interval of 60 minutes.**
```
library(RPostgres)
library(DBI)
library(tidyverse)
library(httr2)
library(lubridate)

# Investigate which symbols we can search for ---------------
req <- request("https://alpha-vantage.p.rapidapi.com") %>%
  req_url_path("query") %>%
  req_url_query("keywords" = "Tesla",
                "function" = "SYMBOL_SEARCH",
                "datatype" = "json") %>%
  req_headers('X-RapidAPI-Key' = '87104e25e5msh06c8b5c6abc80bcp1091e6jsn97e552e79980',
              'X-RapidAPI-Host' = 'alpha-vantage.p.rapidapi.com')

resp <- req %>% 
  req_perform() 
symbols <- resp %>%
  resp_body_json()

symbols$bestMatches[[1]]
symbols$bestMatches[[2]]
```

**6. Transform the fetched data into an R data.frame where time is given in UTC. Create a PostgreSQL table with the columns: ”id”, ”timestamp”, ”close”, and ”volume”, and load the corresponding price data into this table.**
```
# Extract and Transform  ------------------------------------------
# Extract data from Alpha Vantage
req <- request("https://alpha-vantage.p.rapidapi.com") %>%
  req_url_path("query") %>%
  req_url_query("interval" = "60min",
                "function" = "TIME_SERIES_INTRADAY",
                "symbol" = "TSLA",
                "datatype" = "json",
                "output_size" = "compact") %>%
  req_headers('X-RapidAPI-Key' = '87104e25e5msh06c8b5c6abc80bcp1091e6jsn97e552e79980',
              'X-RapidAPI-Host' = 'alpha-vantage.p.rapidapi.com') 

resp <- req %>% 
  req_perform() 
dat <- resp %>%
  resp_body_json()

# TRANSFORM timestamp to UTC time
timestamp <- lubridate::ymd_hms(names(dat$`Time Series (60min)`), tz = "US/Eastern")
timestamp <- format(timestamp, tz = "UTC")

# Prepare data.frame to hold results
df <- tibble(timestamp = timestamp,
                 open = NA, high = NA, low = NA, close = NA, volume = NA)

# TRANSFORM data into a data.frame
for (i in 1:nrow(df)) {
  df[i,-1] <- as.data.frame(dat$`Time Series (60min)`[[i]])
}

# Create table in Postgres ------------------------------------------------
# Put the credentials in this script
# Never push credentials to git!! --> use .gitignore on .credentials.R
source(".credentials.R")

# Function to send queries to Postgres
source("psql_queries.R")

# Create a new schema in Postgres on docker
psql_manipulate(cred = cred_psql_docker, 
                query_string = "CREATE SCHEMA intg2;")

# Create a table in the new schema 
psql_manipulate(cred = cred_psql_docker, 
                query_string = 
                  "create table intg2.prices (
	id serial primary key,
	timestamp timestamp(0) without time zone ,
	open numeric(30,4),
	high numeric(30,4),
	low numeric(30,4),
	close numeric(30,4),
	volume numeric(30,4));")

# LOAD price data -------------------------------
psql_append_df(cred = cred_psql_docker,
               schema_name = "intg2",
               tab_name = "prices",
               df = df)

# Check results -----------------------------------------------------------
# Check that we can fetch the data again
psql_select(cred = cred_psql_docker, 
            query_string = 
              "select * from intg2.prices")

# If you wish, your can delete the schema (all the price data) from Postgres 
psql_manipulate(cred = cred_psql_docker, 
                query_string = "drop SCHEMA intg2 cascade;")
```






