https://s3.console.aws.amazon.com/s3/buckets/crawler-public-ap-northeast-1/flight/

s3://crawler-public-ap-northeast-1/flight/2016/csv/


Steps:
## 1. Play with the original data
## 1-1. Crawl the original data by creating a table - flight_data_original.origin_csv - by glue crawler GUI
```
s3://crawler-public-ap-northeast-1/flight/2016/csv/
```

## 1-2. Query the data
```
select count(1) from origin_csv
```

## 2. Import into private s3 bucket as csv
## 2-1. Run ETL Job into private csv data on S3 - by glue crawler GUI
```
s3://glue-output-ap-northeast-1/flight_csv/
```

## 2-2. Crawl to create a table - flight_data.flight_csv
```
s3://glue-output-ap-northeast-1/flight_csv/
```

## 2-3. Query the data
```
select count(1) from flight_csv
```

## 3. Import into private s3 bucket as parquet
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