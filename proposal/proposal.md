# Dataset Proposal: Analysis of 2023 New York City Yellow Taxi Trips

## 1. Dataset name and source
The proposed primary dataset is the 2023 New York City Yellow Taxi Trip Record Data, published by the New York City Taxi and Limousine Commission (NYC TLC). Each row represents a completed yellow taxi trip and records when and where the trip occurred, its distance, passenger count, payment method and itemised fare information.

Official source URLs:

- NYC TLC Trip Record Data   
    https://www.nyc.gov/site/tlc/about/tlc-trip-record-data.page 

- 2023 Yellow Taxi Trip Data on NYC Open Data  
    https://data.cityofnewyork.us/Transportation/2023-Yellow-Taxi-Trip-Data/4b4i-vvec 

- Yellow Taxi Trip Data Dictionary  
    https://www.nyc.gov/assets/tlc/downloads/pdf/data_dictionary_trip_records_yellow.pdf 

- NYC Taxi Zone Lookup Table  
    https://d37ci6vzurychx.cloudfront.net/misc/taxi_zone_lookup.csv 

The Taxi Zone Lookup CSV originates from the NYC TLC and is linked from the official Trip Record Data page under its taxi zone maps and lookup table resources. The LocationID values in this file correspond to the PULocationID and DOLocationID fields in the trip data.

Direct monthly Parquet files
The following 12 monthly files will be loaded as the 2023 trip dataset:

- January 2023  
    https://d37ci6vzurychx.cloudfront.net/trip-data/yellow_tripdata_2023-01.parquet 

- February 2023  
    https://d37ci6vzurychx.cloudfront.net/trip-data/yellow_tripdata_2023-02.parquet 

- March 2023  
    https://d37ci6vzurychx.cloudfront.net/trip-data/yellow_tripdata_2023-03.parquet 

- April 2023  
    https://d37ci6vzurychx.cloudfront.net/trip-data/yellow_tripdata_2023-04.parquet 

- May 2023  
    https://d37ci6vzurychx.cloudfront.net/trip-data/yellow_tripdata_2023-05.parquet 

- June 2023  
    https://d37ci6vzurychx.cloudfront.net/trip-data/yellow_tripdata_2023-06.parquet 

- July 2023  
    https://d37ci6vzurychx.cloudfront.net/trip-data/yellow_tripdata_2023-07.parquet 

- August 2023  
    https://d37ci6vzurychx.cloudfront.net/trip-data/yellow_tripdata_2023-08.parquet 

- September 2023  
    https://d37ci6vzurychx.cloudfront.net/trip-data/yellow_tripdata_2023-09.parquet 

- October 2023  
    https://d37ci6vzurychx.cloudfront.net/trip-data/yellow_tripdata_2023-10.parquet 

- November 2023  
    https://d37ci6vzurychx.cloudfront.net/trip-data/yellow_tripdata_2023-11.parquet 

- December 2023  
    https://d37ci6vzurychx.cloudfront.net/trip-data/yellow_tripdata_2023-12.parquet 
	
The data is publicly accessible through NYC Open Data and is subject to the applicable NYC Open Data terms of use. The TLC notes that trip records are submitted by authorised technology service providers and may contain data quality limitations. These limitations will be acknowledged and examined during data preparation.

## 2. Dataset scope and size
| Source	|   Approximate row count |	Approximate file size   |
| --- | ---: | ---: |
| 2023 Yellow Taxi Trip Records |   38,310,226  |   635.7 MB compressed |
| Taxi Zone Lookup Table    |   265 |   Less than 20 KB |
| Total source records  |   Approximately 38.31 million |   Approximately 635.7 MB  |

The trip-data file size is approximately 635.7 MB using decimal units, equivalent to approximately 606.3 MiB. The measured size could change slightly if the publisher corrects or republishes a monthly file.

This volume substantially exceeds the recommended minimum of 100,000 rows. It is sufficient to demonstrate distributed loading, partitioned processing, joins, multi-level aggregations, window calculations, shuffling, caching and query optimisation in Apache Spark.

The 2023 calendar year was selected as a fixed and reproducible 12 month dataset whose source files, schema variations, row count and file size have already been verified. It is a complete, established and well documented annual dataset that provides sufficient scale for joins, shuffles, partitioning experiments and execution time benchmarking while supporting complete annual, weekday and hourly analysis. The dedicated NYC Open Data record has also been available for longer than the equivalent 2024 record, providing a more mature and reproducible source for the assessment.

## 3. Column inventory
The table below lists the source and derived columns that will be used in the analysis. The stated data types represent the canonical Spark schema after the monthly files have been standardised.

| Dataset or category | Column | Spark data type | Description |
| --- | --- | --- | --- |
| Yellow Taxi Trips | `VendorID` | `LongType` | Code identifying the technology provider that supplied the trip record. |
| Yellow Taxi Trips | `tpep_pickup_datetime` | `TimestampType` | Date and time when the taxi meter was engaged. |
| Yellow Taxi Trips | `tpep_dropoff_datetime` | `TimestampType` | Date and time when the taxi meter was disengaged. |
| Yellow Taxi Trips | `passenger_count` | `LongType` | Number of passengers reported by the driver. |
| Yellow Taxi Trips | `trip_distance` | `DoubleType` | Trip distance reported by the taxi meter, measured in miles. |
| Yellow Taxi Trips | `RatecodeID` | `LongType` | Rate category applied to the trip, such as standard, JFK or negotiated fare. |
| Yellow Taxi Trips | `store_and_fwd_flag` | `StringType` | Indicates whether the record was temporarily stored in the vehicle before transmission. |
| Yellow Taxi Trips | `PULocationID` | `LongType` | TLC taxi zone identifier for the pickup location. |
| Yellow Taxi Trips | `DOLocationID` | `LongType` | TLC taxi zone identifier for the drop off location. |
| Yellow Taxi Trips | `payment_type` | `LongType` | Code identifying the passenger’s payment method. |
| Yellow Taxi Trips | `fare_amount` | `DoubleType` | Time and distance fare calculated by the taxi meter. |
| Yellow Taxi Trips | `extra` | `DoubleType` | Additional charges, such as overnight or rush hour charges. |
| Yellow Taxi Trips | `mta_tax` | `DoubleType` | Metropolitan Transportation Authority tax applied to eligible trips. |
| Yellow Taxi Trips | `tip_amount` | `DoubleType` | Tip recorded for credit card payments. Cash tips are not recorded. |
| Yellow Taxi Trips | `tolls_amount` | `DoubleType` | Total toll charges associated with the trip. |
| Yellow Taxi Trips | `improvement_surcharge` | `DoubleType` | Improvement surcharge applied to eligible trips. |
| Yellow Taxi Trips | `total_amount` | `DoubleType` | Total passenger charge, including applicable taxes, tips, tolls and surcharges. |
| Yellow Taxi Trips | `congestion_surcharge` | `DoubleType` | Congestion surcharge applied to eligible trips. |
| Yellow Taxi Trips | `airport_fee` | `DoubleType` | Airport pickup or drop off fee applied to eligible trips. |
| Taxi Zone Lookup | `LocationID` | `IntegerType` | Unique taxi zone identifier used to join the lookup to the trip records. |
| Taxi Zone Lookup | `Borough` | `StringType` | Borough containing the taxi zone, such as Manhattan, Queens or Brooklyn. |
| Taxi Zone Lookup | `Zone` | `StringType` | Descriptive name of the taxi zone. |
| Taxi Zone Lookup | `service_zone` | `StringType` | TLC service classification, such as Yellow Zone, Boro Zone or Airports. |
| Derived analytical field | `pickup_date` | `DateType` | Calendar date derived from the pickup timestamp. |
| Derived analytical field | `pickup_hour` | `IntegerType` | Pickup hour represented as an integer from 0 to 23. |
| Derived analytical field | `day_of_week` | `IntegerType` | Day of week number derived from the pickup timestamp. |
| Derived analytical field | `trip_duration_minutes` | `DoubleType` | Time between pickup and drop off, measured in minutes. |
| Derived analytical field | `route_id` | `LongType` | Numerical route identifier constructed from the pickup and drop off location IDs. |
| Derived analytical field | `pickup_epoch_seconds` | `LongType` | Pickup timestamp converted to Unix epoch seconds for partitioning experiments. |
| Derived analytical field | `route_speed_mph` | `DoubleType` | Aggregate route distance divided by aggregate route duration in hours. |
| Derived analytical field | `fare_per_occupied_minute` | `DoubleType` | Aggregate metered fare divided by aggregate trip duration in minutes. |
| Derived analytical field | `speed_deficit_percentage` | `DoubleType` | Percentage by which a route’s speed falls below its corresponding peer benchmark. |
| Derived analytical field | `route_rank` | `IntegerType` | Route position within its pickup borough, weekday and hourly window. |

The prefix tpep refers to the TLC’s Taxicab Passenger Enhancement Program naming convention.

The monthly Parquet files contain minor physical schema differences. Some integer columns use different integer widths, while the airport-fee field appears as airport_fee in January and Airport_fee in later files. Explicit source schemas will therefore be applied before the fields are renamed, cast to the canonical types shown above and combined using unionByName().

The Taxi Zone Lookup Table will be joined to the trip records twice. The pickup join will use PULocationID = LocationID, while the drop-off join will use DOLocationID = LocationID.

The Spark session timezone will be set to America/New York before deriving date, weekday and hourly fields because the TLC timestamps represent New York local time.

## 4. High cardinality column for partitioning experiments
The high cardinality numerical column selected for the partitioning experiment is pickup_epoch_seconds. It will be materialised by converting tpep_pickup_datetime into Unix epoch seconds and storing the result as a Spark LongType.

This field is preferable to route_id for the required hash-versus-range comparison because epoch seconds have a meaningful chronological order. Hash partitioning will distribute timestamp values according to Spark’s hash function, while range partitioning will assign contiguous pickup time ranges to partitions.

The following strategies will be compared using the same number of partitions:  
- df.repartition(n, "pickup_epoch_seconds")
- df.repartitionByRange(n, "pickup_epoch_seconds")

The value of n will be selected after recording the execution environment, available processor cores, input size and sc.defaultParallelism. The choice will aim to provide multiple tasks per available core without creating excessively small partitions.

The number of records in each partition will be measured using spark_partition_id(). Minimum, maximum and average partition counts may also be reported to quantify imbalance. For range partitioning, the minimum and maximum pickup time in each partition may be included to demonstrate temporal locality.

The derived route_id will remain useful for route aggregation and ranking but will not be the principal partitioning column.

## 5. Proposed business case and analytical question
Taxi operators and transport planners need to identify routes that perform poorly relative to comparable traffic conditions. A route should not be classified as inefficient simply because the entire pickup borough was congested at that time.

The proposed analysis will answer the following question:

Within each pickup borough, day of the week and hour of the day, which three high volume pickup–drop-off routes have the greatest travel speed deficit compared with other trips operating in the same borough and time period?

The analysis will also examine whether these routes generate lower metered fare per occupied minute than the corresponding peer benchmark. This provides a combined view of passenger demand, travel efficiency and fare performance.

A route group will be considered high volume when it contains at least 100 valid trips within the relevant borough, weekday and hourly period. This post aggregation threshold will reduce unstable comparisons based on very small groups. The threshold will be declared as a parameter so that its effect can be reviewed during implementation.

## 6. Suitability for distributed analytics
The dataset is suitable for distributed analytics because it contains approximately 38.3 million trip records spread across 12 Parquet files. The complete analysis requires explicit schema handling, filtering, two geographical joins, multiple derived columns, separate route level and borough level aggregations, a join between derived aggregates and partitioned window ranking.

The Taxi Zone Lookup Table also provides a clear opportunity to compare a conventional join with a broadcast join. Broadcasting approximately 265 lookup rows should avoid shuffling the much larger trip dataset for those joins. The physical execution plan will be examined to confirm whether Spark selects or applies the intended broadcast strategy.

The data is likely to have uneven geographical and temporal distributions. Manhattan zones and peak travel periods will contain substantially more trips than less active zones and hours. This makes the dataset appropriate for investigating partition imbalance, data skew, task duration and shuffle behaviour.

The Parquet format supports column pruning and predicate pushdown. Filtering invalid or out-of-scope records before expensive joins and projecting only necessary columns should reduce the amount of data processed in later stages. Join ordering, broadcast joins, persistence and Adaptive Query Execution can then be evaluated using empirical evidence.

## 7. Why a simple GROUP BY is insufficient
A single GROUP BY could calculate trip counts or average fares for each route, but it could not answer the complete business question. The proposed query requires:  
- Two joins to obtain pickup and drop off geographical attributes
- Duration, time period and route derivations
- Multi-level route aggregation
- Post aggregation volume filtering
- A separate borough and time benchmark
- A join between route and benchmark aggregates
- Weighted performance measures
- Comparison with peer performance
- Window ranking within each borough and time period
- Filtering to the highest ranked routes

The result therefore depends on multiple transformation, join, aggregation, shuffle and window processing stages.

## 8. Potential use for later predictive analysis
The dataset contains temporal, geographical, categorical and numerical fields that may support later machine learning analysis. 

| Potential task | Potentially useful fields |
| --- | --- |
| Trip duration prediction | Pickup timestamp, pickup hour, weekday, pickup and drop off locations, passenger count and rate code |
| Fare prediction | Pickup and drop off locations, time attributes, rate code, passenger count, distance and duration |
| Tip behaviour classification | Payment type, fare amount, distance, duration, passenger count, time attributes and location fields |
| Demand forecasting | Pickup date, pickup hour, weekday, pickup zone, borough and historical trip volume |
| Anomaly detection | Duration, distance, speed, fare amount, total amount, tolls and surcharges |

Predictive modelling will require careful separation of predictor and outcome variables. For example, tpep_dropoff_datetime should not be used to predict trip duration because it directly determines the target. Similarly, tip_amount and total_amount should not be used as predictors when tipping behaviour is the outcome.

## Reference List
New York City Taxi and Limousine Commission. _TLC Trip Record Data._  
https://www.nyc.gov/site/tlc/about/tlc-trip-record-data.page 

New York City Taxi and Limousine Commission. _Yellow Taxi Trip Records Data Dictionary._  
https://www.nyc.gov/assets/tlc/downloads/pdf/data_dictionary_trip_records_yellow.pdf

NYC Open Data. _2023 Yellow Taxi Trip Data._  
https://data.cityofnewyork.us/Transportation/2023-Yellow-Taxi-Trip-Data/4b4i-vvec 

New York City Taxi and Limousine Commission. _Taxi Zone Lookup Table._  
https://d37ci6vzurychx.cloudfront.net/misc/taxi_zone_lookup.csv 