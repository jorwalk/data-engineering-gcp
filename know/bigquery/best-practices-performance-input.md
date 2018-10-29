Managing Input Data and Data Sources
====================================

When evaluating your input data, consider the required I/O. How many bytes does your query read? Are you properly limiting the amount of input data? Is your data in native BigQuery storage or an external data source? The amount of data read by a query and the source of the data impact query performance and cost.

You can examine how the input data is read by a query by using the [query plan explanation](https://cloud.google.com/bigquery/query-plan-explanation).

The following best practices provide guidance on controlling your input data and on choosing a data source.

Control projection - Avoid `SELECT *`
-------------------------------------

**Best practice:** Control projection - Query only the columns that you need.

Projection refers to the number of columns that are read by your query. Projecting excess columns incurs additional (wasted) I/O and materialization (writing results).

Using `SELECT *` is the most expensive way to query data. When you use `SELECT *`, BigQuery does a full scan of every column in the table.

If you are experimenting with data or exploring data, use one of the [data preview options](https://cloud.google.com/bigquery/docs/best-practices-costs#preview-data) instead of `SELECT *`.

Applying a `LIMIT` clause to a `SELECT *` query does not affect the amount of data read. You are billed for reading all bytes in the entire table, and the query counts against your free tier quota.

Instead, query only the columns you need. For example, use `SELECT * EXCEPT` to exclude one or more columns from the results.

If you do require queries against every column in a table, but only against a subset of data, consider:

-   Materializing results in a destination table and querying that table instead
-   [Partitioning your tables by date](https://cloud.google.com/bigquery/docs/creating-partitioned-tables) and querying the relevant partition; for example, `WHERE _PARTITIONDATE="2017-01-01"` only scans the January 1, 2017 partition

Querying a subset of data or using `SELECT * EXCEPT` can greatly reduce the amount of data that is read by a query. In addition to the cost savings, performance is improved by reducing the amount of data I/O and the amount of materialization that is required for the query results.

Prune partitioned queries
-------------------------

**Best practice:** When querying a [time-partitioned table](https://cloud.google.com/bigquery/docs/querying-partitioned-tables), use the `_PARTITIONTIME` pseudo column to filter the partitions.

When you query partitioned tables, use the `_PARTITIONTIME` pseudo column. Filtering the data using `_PARTITIONTIME`allows you to specify a date or range of dates. For example, the following `WHERE` clause uses the `_PARTITIONTIME`pseudo column to specify partitions between January 1, 2016 and January 31, 2016:
```
WHERE _PARTITIONTIME
BETWEEN TIMESTAMP("20160101")
    AND TIMESTAMP("20160131")
```
The query processes data only in the partitions that are indicated by the date range, reducing the amount of input data. Filtering your partitions improves query performance and reduces costs.

Denormalize data whenever possible
----------------------------------

**Best practice:** BigQuery performs best when your data is denormalized. Rather than preserving a relational schema such as a star or snowflake schema, denormalize your data and take advantage of nested and repeated fields. Nested and repeated fields can maintain relationships without the performance impact of preserving a relational (normalized) schema.

The storage savings from normalized data are less of a concern in modern systems. Increases in storage costs are worth the performance gains from denormalizing data. Joins require data coordination (communication bandwidth). Denormalization localizes the data to individual [slots](https://cloud.google.com/bigquery/docs/slots) so execution can be done in parallel.

If you need to maintain relationships while denormalizing your data, use nested and repeated fields instead of completely flattening your data. When relational data is completely flattened, network communication (shuffling) can negatively impact query performance.

For example, denormalizing an orders schema without using nested and repeated fields may require you to group by a field like `order_id` (when there is a one-to-many relationship). Because of the shuffling involved, grouping the data is less performant than denormalizing the data using nested and repeated fields.

In some circumstances, denormalizing your data and using nested and repeated fields may not result in increased performance. Avoid denormalization in these use cases:

-   You have a star schema with frequently changing dimensions.
-   BigQuery complements an Online Transaction Processing (OLTP) system with row-level mutation, but can't replace it.

### Using nested and repeated fields

BigQuery doesn't require a completely flat denormalization. You can use nested and repeated fields to maintain relationships.

-   Nesting data (STRUCT)

    -   Nesting data allows you to represent foreign entities inline.
    -   Querying nested data uses "dot" syntax to reference leaf fields, which is similar to the syntax using a join.
    -   Nested data is represented as a [STRUCT type](https://cloud.google.com/bigquery/docs/reference/standard-sql/data-types#struct-type) in standard SQL.
-   Repeated data (ARRAY)

    -   Creating a field of type RECORD with the mode set to REPEATED allows you to preserve a 1:many relationship inline (so long as the relationship isn't high cardinality).
    -   With repeated data, shuffling is not necessary.
    -   Repeated data is represented as an ARRAY. You can use an [ARRAY function](https://cloud.google.com/bigquery/docs/reference/standard-sql/functions-and-operators#array-functions) in standard SQL when you query the repeated data.
-   Nested and repeated data (ARRAY of STRUCTs)

    -   Nesting and repetition complement each other.
    -   For example, in a table of transaction records, you could include an array of line item STRUCTs.

Use external data sources appropriately
---------------------------------------

**Best practice:** If query performance is a top priority, do not use an external data source.

Querying tables in BigQuery managed storage is typically much faster than querying external tables in Google Cloud Storage, Google Drive, or Google Cloud Bigtable.

Use an external data source for these use cases:

-   Performing extract, transform, and load (ETL) operations when loading data
-   Frequently changing data
-   Periodic loads such as recurring ingestion of data from Cloud Bigtable

Avoid excessive wildcard tables
-------------------------------

**Best practice:** When querying [wildcard tables](https://cloud.google.com/bigquery/docs/querying-wildcard-tables), use the most granular prefix possible.

Use wildcards to query multiple tables by using concise SQL statements. Wildcard tables are a union of tables that match the wildcard expression. Wildcard tables are useful if your dataset contains:

-   Multiple, similarly named tables with compatible schemas
-   Sharded tables

**Note:** If your data allows it, use time-partitioned tables instead of sharded tables. For more information, see [Avoid oversharding tables](https://cloud.google.com/bigquery/docs/best-practices-performance-communication#avoid_oversharding_tables).

When you query a wildcard table, specify a wildcard (*) after the common table prefix. For example, `FROM `bigquery-public-data.noaa_gsod.gsod194*`` queries all tables from the 1940s.

More granular prefixes perform better than shorter prefixes. For example, `FROM `bigquery-public-data.noaa_gsod.gsod194*`` performs better than `FROM `bigquery-public-data.noaa_gsod.*`` because fewer tables match the wildcard.
