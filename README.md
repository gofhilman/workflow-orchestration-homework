# workflow-orchestration-homework

Homework solution for Workflow Orchestration

Problems: <https://github.com/DataTalksClub/data-engineering-zoomcamp/blob/main/cohorts/2026/02-workflow-orchestration/homework.md>

1. Within the execution for Yellow Taxi data for the year 2020 and month 12, the uncompressed file size is 128.3 MiB.

    Set up kestra using `docker-compose.yaml` by providing the Google Cloud Platform service account secrets in `.env_encoded`. Then run kestra flow `kestra-flows/01-gcp-kv.yaml`, `kestra-flows/02-gcp-setup.yaml`, and `kestra-flows/03-gcp-taxi.yaml` simultaneously by using `yellow` taxi, `2020` year, and `12` month inputs. The output file size of the extract task is found to be 128.3 MiB.

2. The rendered value of the variable file when the inputs taxi is set to green, year is set to 2020, and month is set to 04 during execution is `green_tripdata_2020-04.csv`.

3. There are 24,648,499 rows for the Yellow Taxi data for all CSV files in the year 2020.

    First, run kestra flow `kestra-flows/04-gcp-loop-taxi.yaml` by using `yellow` `taxi_types`, `2020` `years`, and all `months` inputs. Then run kestra flow `kestra-flows/05-gcp-yearly-tripdata.yaml` by using `yellow` `taxi_type` and`2020` `year` to utilize kestra to query in GCP using its template engine (pebble). The result shows the `row_count` is 24,648,499.

    Alternative:

    ```sql
    CREATE OR REPLACE EXTERNAL TABLE `even-lyceum-282418.zoomcamp.yellow_tripdata_2020_ext`
    OPTIONS (
        format = 'CSV',
        uris = ['gs://gofhilman-zoomcamp/yellow_tripdata_2020-*.csv']
    );
    
    SELECT COUNT(*) FROM `even-lyceum-282418.zoomcamp.yellow_tripdata_2020_ext`;
    ```

4. There are 1,734,051 rows for the Yellow Taxi data for all CSV files in the year 2020.

    Use the same method as number 3 solution, but change the taxi type to be `green`.

5. There are 1,925,152 rows for the Yellow Taxi data for the March 2021 CSV file.

    This one is easier, just run `kestra-flows/03-gcp-taxi.yaml` by using `yellow` taxi, `2021` year, and `03` month inputs. Then query the table row number in BigQuery:

    ```sql
    SELECT 
        COUNT(1) AS row_count 
    FROM 
        `even-lyceum-282418.zoomcamp.yellow_tripdata_2021_03_ext`;
    ```

    The result shows the `row_count` is 1,925,152.

6. To configure the timezone to New York in a Schedule trigger is by adding a `timezone` property set to `America/New_York` in the `Schedule` trigger configuration.
