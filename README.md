# ITO5202 Assessment 1

Student ID: 27602966

Unit: ITO5202 Data Processing for Big Data TP5-26

## Dataset

This assessment uses the 2023 NYC TLC Yellow Taxi Trip Record Data together with the Taxi Zone Lookup table.

The trip data consists of the twelve monthly 2023 Parquet files. The Taxi Zone Lookup is used to associate pickup and dropoff location IDs with borough and zone information.

Dataset source:  
- NYC Taxi and Limousine Commission Trip Record Data  
https://www.nyc.gov/site/tlc/about/tlc-trip-record-data.page

- NYC Taxi Zone Lookup Table  
https://d37ci6vzurychx.cloudfront.net/misc/taxi_zone_lookup.csv

## Project

This repository contains the implementation for Assessment 1: Analysing historical data with system performance.

The analysis uses Apache Spark to process the 2023 New York City Yellow Taxi Trip Record Data.

## Environment

The analysis was completed using the Monash ITO5202 Docker image:   `monashfit/ito5202-pyspark:4`

Spark was run in local mode with:   `local[2]`

Driver memory:  `2g`

Docker was configured with approximately 5 GB memory and 2 GB swap.

## Repository Structure

`assessment1.ipynb` contains the main analysis.

`proposal/proposal.md` contains the approved dataset proposal.

`data/` contains the local dataset files and is excluded from GitHub.

`dag.png` contains the Spark Web UI DAG screenshot used in the notebook.

## Running the Notebook

1. Download the twelve 2023 Yellow Taxi Parquet files and the Taxi Zone Lookup file into the `data` folder.

2. Start the ITO5202 Docker container.

3. Open JupyterLab.

4. Open `assessment1.ipynb`.

5. Run the notebook cells in order.

The `data` folder is excluded from Git because the source datasets are too large to store in the repository.
