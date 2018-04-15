https://s3.console.aws.amazon.com/s3/buckets/crawler-public-ap-northeast-1/flight/

s3://crawler-public-ap-northeast-1/flight/2016/csv/


Steps:
## 1. Play with the original data
## 1-1. Crawl the original data by creating a table - flight_data.csv - by the AWS Glue Crawlers GUI

* Data store: s3://crawler-public-ap-northeast-1/flight/2016/csv/
* Crawler name: crawl-public-flight-2016-csv
* Choose an existing IAM role: AWSGlueServiceRole created by CloudFormation
```
glue-IAMStack-1JAF96FXYCZR3-AWSGlueServiceRole-15QOHKS1N9IBY
```
* Output Database: flight_data

## 1-2. Query the data at Athena
```
select count(1) from csv
```

## 2. Import into private s3 bucket as csv
## 2-1. Run ETL Job into private csv data on S3 by the AWS Glue Jobs GUI

* Name: job_import_csv
* IAM Role: AWSGlueServiceRole created by CloudFormation
```
glue-IAMStack-1JAF96FXYCZR3-AWSGlueServiceRole-15QOHKS1N9IBY
```
* Script: Use the proposed script by AWS Glue
* Script file name: job_import_csv
* Data source: csv - create at Step 1-1
* Data target: Create a gzip-compressed CSV table in your own S3 table
```
s3://glue-output-ap-northeast-1/flight_csv/
```

## 2-2. Crawl to create a table - flight_data.private_csv

* Data store: s3://glue-output-ap-northeast-1/flight_csv/
* Crawler name: crawl-private-flight-2016-csv
* Choose an existing IAM role: AWSGlueServiceRole created by CloudFormation
```
glue-IAMStack-1JAF96FXYCZR3-AWSGlueServiceRole-15QOHKS1N9IBY
```
* Output Database: flight_data
* Prefix: private_

## 2-3. Query the data
```
select count(1) from private_csv
```

## 3. ETL to Parquet
## 3-1. Run ETL Job to convert flight_data.flight_csv into private parquet data on S3 - by glue crawler GUI, only data for January 
```
s3://glue-output-ap-northeast-1/flight_parquet/
```

## 2-2. Crawl to create a table - flight_data.flight_parquet
```
s3://glue-output-ap-northeast-1/flight_parquet/
```

## 2-3. Query the data
```
select count(1) from flight_parquet
```