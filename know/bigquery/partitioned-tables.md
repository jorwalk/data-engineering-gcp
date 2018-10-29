Introduction to Partitioned Tables
==================================

This page provides an overview of partitioned table support in BigQuery.

A partitioned table is a special table that is divided into segments, called partitions, that make it easier to manage and query your data. By dividing a large table into smaller partitions, you can improve query performance, and you can control costs by reducing the number of bytes read by a query.

There are two types of table partitioning in BigQuery:

-   **Tables partitioned by ingestion time**: Tables partitioned based on the data's ingestion (load) date or arrival date.
-   **Partitioned tables**: Tables that are partitioned based on a [`TIMESTAMP`](https://cloud.google.com/bigquery/docs/reference/standard-sql/data-types#timestamp-type) or [`DATE`](https://cloud.google.com/bigquery/docs/reference/standard-sql/data-types#date-type) column.

### Ingestion-time partitioned tables

When you create a table partitioned by ingestion time, BigQuery automatically loads data into daily, date-based partitions that reflect the data's ingestion or arrival date. Pseudo column and suffix identifiers allow you to restate (replace) and redirect data to partitions for a specific day.

Ingestion-time partitioned tables include a pseudo column named `_PARTITIONTIME` that contains a date-based timestamp for data that is loaded into the table. Queries against time-partitioned tables can restrict the data read by supplying `_PARTITIONTIME` filters that represent a partition's location. All the data in the specified partition is read by the query, but the `_PARTITIONTIME` predicate filter restricts the number of partitions scanned.

When you create ingestion-time partitioned tables, the partitions have the same schema definition as the table. If you need to load data into a partition with a schema that is not the same as the schema of the table, you must update the schema of the table before loading the data. Alternatively, you can use [schema update options](https://cloud.google.com/bigquery/docs/managing-table-schemas) to update the schema of the table in a load job or query job.

### Partitioned tables

BigQuery also allows partitioned tables. Partitioned tables allow you to bind the partitioning scheme to a specific`TIMESTAMP` or `DATE` column. Data written to a partitioned table is automatically delivered to the appropriate partition based on the date value (expressed in UTC) in the partitioning column.

Partitioned tables do not need a `_PARTITIONTIME` pseudo column. Queries against partitioned tables can specify predicate filters based on the partitioning column to reduce the amount of data scanned.

When you create partitioned tables, two special partitions are created:

-   The `__NULL__` partition --- represents rows with `NULL` values in the partitioning column
-   The `__UNPARTITIONED__` partition --- represents data that exists outside the allowed range of dates

With the exception of the `__NULL__` and `__UNPARTITIONED__` partitions, all data in the partitioning column matches the date of the partition identifier. This allows a query to determine which partitions contain no data that satisfies the filter conditions. Queries that filter data on the partitioning column can restrict values and completely prune unnecessary partitions.

### Partitioning versus sharding

As an alternative to partitioned tables, you can shard tables using a time-based naming approach such as `[PREFIX]_YYYYMMDD`. This is referred to as creating date-sharded tables. Using either standard SQL or legacy SQL, you can specify a query with a `UNION` operator to limit the tables scanned by the query.

Partitioned tables perform better than tables sharded by date. When you create date-named tables, BigQuery must maintain a copy of the schema and metadata for each date-named table. Also, when date-named tables are used, BigQuery might be required to verify permissions for each queried table. This practice also adds to query overhead and impacts query performance. The recommended best practice is to use partitioned tables instead of date-sharded tables.

### Comparing partitioning options

The following table compares sharded tables and partitioned tables.

| Capability | Sharded tables | Ingestion-time partitioned tables | Partitioned tables |
| Partitioning method | None: Sharding tables and querying them using a [`UNION`](https://cloud.google.com/bigquery/docs/reference/standard-sql/query-syntax#union)operator can simulate partitioning. | Partitioned based on the ingestion or arrival date of the data. Partition information can be referenced using a pseudo column. | Partitioned based on data in a specified [`TIMESTAMP`](https://cloud.google.com/bigquery/docs/reference/standard-sql/data-types#timestamp-type) or [`DATE`](https://cloud.google.com/bigquery/docs/reference/standard-sql/data-types#date-type) column. |
| Partition identifiers | None | You can use any valid date between 0001-01-01 and 9999-12-31, but [`DML`](https://cloud.google.com/bigquery/docs/reference/standard-sql/data-manipulation-language) statements cannot reference dates prior to 1970-01-01 or after 2159-12-31. | A valid entry from the bound `DATE` or `TIMESTAMP` column. Currently, date values prior to 1970-01-01 and later than 2159-12-31 are placed in a shared `UNPARTITIONED` partition. `NULL` values reside in an explicit `NULL` partition. |
| Limiting scanned data | Reference only the shards you need and limit the data by excluding unnecessary columns from the query. | Use the `_PARTITIONTIME`pseudo column to prune partitions. | Use predicate filters on the partitioning column. |
| Number of partitions | The number of tables is unrestricted, but queries can only reference up to 1,000 tables. | Up to 4,000 partitions. | Up to 4,000 partitions. |
| Update operations | You are limited to [1,000 updates per day](https://cloud.google.com/bigquery/quotas#standard_tables). | An individual operation can commit to a single partition. The latest partition (by default), or one specified using a partition decorator such as `[TABLE]$[DATE]`. | An individual operation can commit data into up to 2,000 distinct partitions. |
| Streaming inserts | One global buffer for the table. | Using partition suffixes, you can stream to partitions within the last 31 days in the past and 16 days in the future relative to the current date, based on current UTC time. | You can stream data between 1 year in the past and 6 months in the future. Data outside of this range is rejected. When the data is streamed, data between 7 days in the past and 3 days in the future is placed in the streaming buffer, and then it is extracted to the corresponding partitions. Data outside of this window (but inside the 1 year, 6 month range) is placed in the `UNPARTITIONED` partition. When there's enough unpartitioned data, it is loaded to the corresponding partitions. |
| Time zone evaluation | Defined by user semantics | UTC | UTC |

Partitioned table quotas and limits
-----------------------------------

Partitioned tables have defined [limits](https://cloud.google.com/bigquery/quotas#partitioned_tables) in BigQuery.

Quotas and limits also apply to the different types of jobs you can run against partitioned tables, including:

-   [Loading data](https://cloud.google.com/bigquery/quotas#load_jobs) (load jobs)
-   [Exporting data](https://cloud.google.com/bigquery/quotas#export_jobs) (export jobs)
-   [Querying data](https://cloud.google.com/bigquery/quotas#query_jobs) (query jobs)
-   [Copying tables](https://cloud.google.com/bigquery/quotas#copy_jobs) (copy jobs)

For more information on all quotas and limits, see [Quotas and Limits](https://cloud.google.com/bigquery/quotas).

Partitioned table pricing
-------------------------

When you create and use partitioned tables in BigQuery, your charges are based on how much data is stored in the partitions and on the queries you run against the data:

-   For information on storage pricing, see [Storage pricing](https://cloud.google.com/bigquery/pricing#storage).
-   For information on query pricing, see [Query pricing](https://cloud.google.com/bigquery/pricing#queries).

Many partitioned table operations are free, including loading data into partitions, copying partitions, and exporting data from partitions. Though free, these operations are subject to BigQuery's [Quotas and Limits](https://cloud.google.com/bigquery/quotas). For information on all free operations, see [Free operations](https://cloud.google.com/bigquery/pricing#free) on the pricing page.
