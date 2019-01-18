# Weather Data in BigQuery

## Overview
In this lab you will analyze historical weather observations using BigQuery and use weather data in conjunction with other datasets.

### What you'll learn
In this lab, you will:

* Carry out interactive queries on the BigQuery console.
* Combine and run analytics on multiple datasets.

## Prerequisites
This is a fundamental level lab and assumes some experience with BigQuery and SQL. The following lab can get you up to speed with these GCP services:

[BigQuery: Qwik Start - Web User Interface](https://google.qwiklabs.com/catalog_lab/685)

Take these labs first if you have never worked with BigQuery or MySQL, then come back to this one.

## Introduction
This lab uses two public datasets in BigQuery: weather data from NOAA and citizen complaints data from New York City.

You will encounter, for the first time, several aspects of Google Cloud Platform that are of great benefit to scientists:

1. **Serverless.** No need to download data to your machine in order to work with it - the dataset will remain on the cloud.

1. **Ease of use.** Run ad-hoc SQL queries on your dataset without having to prepare the data, like indexes, beforehand. This is invaluable for data exploration.

1. **Scale.** Carry out data exploration on extremely large datasets interactively. You don't need to sample the data in order to work with it in a timely manner.

1. **Shareability.** You will be able to run queries on data from different datasets without any issues. BigQuery is a convenient way to share datasets. Of course, you can also keep your data private, or share them only with specific persons -- not all data need to be public.

The end-result is that you will find what types of municipal complaints are correlated with weather. For example, you will find (not surprisingly) that complaints about residential furnaces are most common when it is cold outside:

![Mean Temperature Daily](https://gcpstaging-qwiklab-website-prod.s3.amazonaws.com/bundles/assets/ecc98f40549cc763bfdef8a575e6fd66c69a02494646cca89636b7807610727c.png)

Explore weather data
--------------------

### Open BigQuery Console

In the Google Cloud Console, select Navigation menu > BigQuery:

![BigQuery_menu.png](https://gcpstaging-qwiklab-website-prod.s3.amazonaws.com/bundles/assets/430517e9a9607573e021386a065801dab9ea1a4a1051400d925df189c0f4eae7.png)

When the console opens, change to the Classic UI.

Click on Go to Classic UI.

![BigQuery_classicUI.png](https://gcpstaging-qwiklab-website-prod.s3.amazonaws.com/bundles/assets/8297f980d0f0604aaac740e81c1dd0fd438e2ec5306f6e783f65a112cefffc4f.png)

The BigQuery console opens in a new browser tab. Select your Qwiklabs project by clicking the Project Down arrow, selecting Switch to Project > Your Qwiklab Project:

![BigQuery_resources_chooseProj.png](https://gcpstaging-qwiklab-website-prod.s3.amazonaws.com/bundles/assets/7a3a1ad9deb7c6a8d1868d222f7b84b815879b63b1cb63fe460f570b69c551e0.png)

The BigQuery console is ready to go! ![BigQuery_console.png](https://gcpstaging-qwiklab-website-prod.s3.amazonaws.com/bundles/assets/3af06038df9ab6190fbb9d5d54b23d44ab1bb9791691d7823c8180709d89b4bb.png)

In the left-hand menu, find and select the table bigquery-public-data:noaa_gsod. Then scroll down and click on the gsod2014 table.

In the Table Details that show on the right-hand pane, click on the Previewtab.

Examine the columns and some of the data values.

Now in the BigQuery Console, click on Compose Query in the top-left corner.

In the query textbox, paste in the following:

```
SELECT
  -- Create a timestamp from the date components.
  stn,
  TIMESTAMP(CONCAT(year,"-",mo,"-",da)) AS timestamp,
  -- Replace numerical null values with actual null
  AVG(IF (temp=9999.9,
      null,
      temp)) AS temperature,
  AVG(IF (wdsp="999.9",
      null,
      CAST(wdsp AS Float64))) AS wind_speed,
  AVG(IF (prcp=99.99,
      0,
      prcp)) AS precipitation
FROM
  `bigquery-public-data.noaa_gsod.gsod20*`
WHERE
  CAST(YEAR AS INT64) > 2010
  AND CAST(MO AS INT64) = 6
  AND CAST(DA AS INT64) = 12
  AND (stn="725030" OR  -- La Guardia
    stn="744860")    -- JFK
GROUP BY
  stn,
  timestamp
ORDER BY
  timestamp DESC,
  stn ASC

```

Under the query box, click the Show Options button. Un-check the Use Legacy SQL box to use Standard SQL:

![84377a359c3f2980.png](https://gcpstaging-qwiklab-website-prod.s3.amazonaws.com/bundles/assets/51ad9593866323dff24edb8c92834409be0b35003757d74303b110a797a9befa.png)

Then click Run Query. Look at the result and try to determine what this query does.

Click x on top right to close the Query editor.

Explore New York citizen complaints data
----------------------------------------

In the BigQuery Console, click the blue down arrow next to your Qwiklabs project name. Select Switch to project > Display project...

![display-project.png](https://gcpstaging-qwiklab-website-prod.s3.amazonaws.com/bundles/assets/68ea28ec9c4f9997cd82e5041b804662f0bf8da85f7b92ad25a01f1bc7855ca2.png)

In the display project panel that pops up, type in `bigquery-public-data`and click OK.

Now select the newly added `bigquery-public-data` project from the left-hand menu and select new_york > 311_service_requests. Then click on the Preview tab. Your console should resemble the following:

![311-request.png](https://gcpstaging-qwiklab-website-prod.s3.amazonaws.com/bundles/assets/443b0189c16e8e3261449943795f8403bcebbda31d515f71d0cdc452e3812a8b.png)

Examine the columns and some of the data values.

Click on Compose Query and enter the following (remember to un-checked the Use Legacy SQL option):

```
SELECT
  EXTRACT(YEAR
  FROM
    created_date) AS year,
  complaint_type,
  COUNT(1) AS num_complaints
FROM
  `bigquery-public-data.new_york.311_service_requests`
GROUP BY
  year,
  complaint_type
ORDER BY
  num_complaints DESC

```

Click Run Query.

Look at the results to determine what the most common complaints are. You will try to determine if these complaints correlate to weather in a later part of this lab.

Saving a new table of weather data
----------------------------------

In the left hand-side of the BigQuery Console, click on the blue arrow next to your project name and select Create new dataset.

![516af07aeb7abbab.png](https://gcpstaging-qwiklab-website-prod.s3.amazonaws.com/bundles/assets/cc1176a7b4804c779f93d6ca20718c1437feac29fd640c40e2917d81d6c94691.png)

Give the dataset the ID "demos" then click OK.

In the BigQuery Console, click Compose Query.

Click on the Show Options button. Click the Select Table button and type in the name "nyc_weather" in the Table ID field.

Click OK.

![e189516fa5bbf17f.png](https://gcpstaging-qwiklab-website-prod.s3.amazonaws.com/bundles/assets/6e8efe3389e5e8a75e5593faf0cd17d1eee49d1666ee572a2b59cfc64cb69edb.png)

Up in the New Query field, enter the following query:

```
SELECT
  -- Create a timestamp from the date components.
  timestamp(concat(year,"-",mo,"-",da)) as timestamp,
  -- Replace numerical null values with actual nulls
  AVG(IF (temp=9999.9, null, temp)) AS temperature,
  AVG(IF (visib=999.9, null, visib)) AS visibility,
  AVG(IF (wdsp="999.9", null, CAST(wdsp AS Float64))) AS wind_speed,
  AVG(IF (gust=999.9, null, gust)) AS wind_gust,
  AVG(IF (prcp=99.99, null, prcp)) AS precipitation,
  AVG(IF (sndp=999.9, null, sndp)) AS snow_depth
FROM
  `bigquery-public-data.noaa_gsod.gsod20*`
WHERE
  CAST(YEAR AS INT64) > 2008
  AND (stn="725030" OR  -- La Guardia
       stn="744860")    -- JFK
GROUP BY timestamp

```

Remember to un-check the Use Legacy SQL box.

Click Run Query.

The results are now saved in the dataset you created (demos).

![1f38ac8a10a6735e.png](https://gcpstaging-qwiklab-website-prod.s3.amazonaws.com/bundles/assets/6b64aff0e757a7b2f9d85e1c9e1347c5828bdc693404ed9e1a6d17e996f9798e.png)

Click on the x next to *Destination table* to remove it for future queries.

Click the x to close the query.

Find correlation between weather and complaints
-----------------------------------------------

Compare the number of complaints and temperature using the [CORR](https://docs.oracle.com/cd/B19306_01/server.102/b14200/functions028.htm)function.

Click Compose Query and run the following query, substituting your GCP Project Number for `YOUR-PROJECT-NUMBER` (remember to un-checked the Use Legacy SQL option):

```
SELECT
  descriptor,
  sum(complaint_count) as total_complaint_count,
  count(temperature) as data_count,
  ROUND(corr(temperature, avg_count),3) AS corr_count,
  ROUND(corr(temperature, avg_pct_count),3) AS corr_pct
From (
SELECT
  avg(pct_count) as avg_pct_count,
  avg(day_count) as avg_count,
  sum(day_count) as complaint_count,
  descriptor,
  temperature
FROM (
  SELECT
    DATE(timestamp) AS date,
    temperature
  FROM
    demos.nyc_weather) a
  JOIN (
  SELECT x.date, descriptor, day_count, day_count / all_calls_count as pct_count
  FROM
    (SELECT
      DATE(created_date) AS date,
      concat(complaint_type, ": ", descriptor) as descriptor,
      COUNT(*) AS day_count
    FROM
      `bigquery-public-data.new_york.311_service_requests`
    GROUP BY
      date,
      descriptor)x
    JOIN (
      SELECT
        DATE(timestamp) AS date,
        COUNT(*) AS all_calls_count
      FROM `<YOUR-PROJECT-NUMBER>.demos.nyc_weather`
      GROUP BY date
    )y
  ON x.date=y.date
)b
ON
  a.date = b.date
GROUP BY
  descriptor,
  temperature
)
GROUP BY descriptor
HAVING
  total_complaint_count > 5000 AND
  ABS(corr_pct) > 0.5 AND
  data_count > 5
ORDER BY
  ABS(corr_pct) DESC

```

Click Run Query.

The results indicate that Heating complaints are negatively correlated with temperature (i.e., more heating calls on cold days) and calls about dead trees are positively correlated with temperature (i.e., more calls on hot days).

Next you'll compare the number of complaints and wind speed with the CORR function.

Click Compose Query and run the following query, substituting your GCP Project Number for `YOUR-PROJECT-NUMBER` (remember to un-check the Legacy SQL option):

```
SELECT
  descriptor,
  sum(complaint_count) as total_complaint_count,
  count(wind_speed) as data_count,
  ROUND(corr(wind_speed, avg_count),3) AS corr_count,
  ROUND(corr(wind_speed, avg_pct_count),3) AS corr_pct
From (
SELECT
  avg(pct_count) as avg_pct_count,
  avg(day_count) as avg_count,
  sum(day_count) as complaint_count,
  descriptor,
  wind_speed
FROM (
  SELECT
    DATE(timestamp) AS date,
    wind_speed
  FROM
    demos.nyc_weather) a
  JOIN (
  SELECT x.date, descriptor, day_count, day_count / all_calls_count as pct_count
  FROM
    (SELECT
      DATE(created_date) AS date,
      concat(complaint_type, ": ", descriptor) as descriptor,
      COUNT(*) AS day_count
    FROM
      `bigquery-public-data.new_york.311_service_requests`
    GROUP BY
      date,
      descriptor)x
    JOIN (
      SELECT
        DATE(timestamp) AS date,
        COUNT(*) AS all_calls_count
      FROM `<YOUR-PROJECT-NUMBER>.demos.nyc_weather`
      GROUP BY date
    )y
  ON x.date=y.date
)b
ON
  a.date = b.date
GROUP BY
  descriptor,
  wind_speed
)
GROUP BY descriptor
HAVING
  total_complaint_count > 5000 AND
  ABS(corr_pct) > 0.5 AND
  data_count > 5
ORDER BY
  ABS(corr_pct) DESC

```

Notice that the Corr columns are both negative for noise related complaints --- do you have a hypothesis for why noise complaints reduce on windy days? Are the coefficients statistically sufficient?

As you can see, BigQuery can give you insights into many different problems from many different angles.

Summary
-------

In this lab you did ad-hoc queries on two datasets. You were able to query the data without setting up any clusters, creating any indexes, etc. You were also able to mash up the two datasets and get some interesting insights. All without ever leaving your browser!
