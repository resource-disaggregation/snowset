# Snowflake Dataset

This repository contains documentation for the dataset that accompanies our NSDI 2020 paper, "Building an Elastic Query Engine on Disaggregated Storage". It also includes scripts to aid with processing of the data and to reproduce the analysis results in the paper.

## Main Dataset

The main dataset contains several statistics (timing, I/O, resource usage, etc..) pertaining to ~70 million queries from all customers that ran on [Snowflake](https://www.snowflake.com/) over a 14 day period from Feb 21st 2018 to March 7th 2018. This dataset is available in both CSV and Parquet formats and can be obtained from the [Downloads](download.md) page.

### Schema

Each row corresponds to one unique query with the columns representing various characteristics pertaining to that query. The **queryId** column contains a unique 64-bit identifier for each query. For a detailed description of all of the columns, please refer to the [Schema](schema.md) page. 

## Auxiliary Time-series Explosion

### Schema

## Scripts

## Usage
