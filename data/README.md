# Dataset Instructions

The dataset files used in this assessment are not stored in the GitHub repository because of their size.

The analysis uses the 2023 New York City Yellow Taxi Trip Record Data published by the New York City Taxi and Limousine Commission, together with the Taxi Zone Lookup table.

## Required files

### 1. 2023 Yellow Taxi monthly Parquet files

Download the twelve monthly Yellow Taxi Parquet files for January to December 2023 using either of the following options.

#### Option 1: NYC TLC source website

Download the monthly files from the official NYC Taxi and Limousine Commission Trip Record Data page:

https://www.nyc.gov/site/tlc/about/tlc-trip-record-data.page

Select the Yellow Taxi Trip Records for January to December 2023.

#### Option 2: Direct monthly download links

January 2023  
https://d37ci6vzurychx.cloudfront.net/trip-data/yellow_tripdata_2023-01.parquet

February 2023  
https://d37ci6vzurychx.cloudfront.net/trip-data/yellow_tripdata_2023-02.parquet

March 2023  
https://d37ci6vzurychx.cloudfront.net/trip-data/yellow_tripdata_2023-03.parquet

April 2023  
https://d37ci6vzurychx.cloudfront.net/trip-data/yellow_tripdata_2023-04.parquet

May 2023  
https://d37ci6vzurychx.cloudfront.net/trip-data/yellow_tripdata_2023-05.parquet

June 2023  
https://d37ci6vzurychx.cloudfront.net/trip-data/yellow_tripdata_2023-06.parquet

July 2023  
https://d37ci6vzurychx.cloudfront.net/trip-data/yellow_tripdata_2023-07.parquet

August 2023  
https://d37ci6vzurychx.cloudfront.net/trip-data/yellow_tripdata_2023-08.parquet

September 2023  
https://d37ci6vzurychx.cloudfront.net/trip-data/yellow_tripdata_2023-09.parquet

October 2023  
https://d37ci6vzurychx.cloudfront.net/trip-data/yellow_tripdata_2023-10.parquet

November 2023  
https://d37ci6vzurychx.cloudfront.net/trip-data/yellow_tripdata_2023-11.parquet

December 2023  
https://d37ci6vzurychx.cloudfront.net/trip-data/yellow_tripdata_2023-12.parquet

The monthly files should use the following naming convention:

`yellow_tripdata_2023-MM.parquet`

where `MM` represents the month from `01` to `12`.

### 2. Taxi Zone Lookup

Download the Taxi Zone Lookup file from:

https://d37ci6vzurychx.cloudfront.net/misc/taxi_zone_lookup.csv

The file should be named:

`taxi_zone_lookup.csv`

## File placement

Place all thirteen files directly inside the `data` folder before running `assessment1.ipynb`.