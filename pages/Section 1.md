# 1. Initial Exploration of the Global Macro Database (GMD)


To begin, let's explore the database we'll be working with to understand its data, possibilities, and limitations. This will ensure our conclusions are well-founded and our action plans are effective.


Let's examine the columns available to us.
```sql gmd_structure
SELECT column_name
FROM information_schema.columns
WHERE table_name = 'gmd'
ORDER BY ordinal_position;
```
Let's explore some key data indicators.


```sql columns_exploration
SELECT
  COUNT(DISTINCT countryname) AS total_countries,
  COUNT(DISTINCT year) AS total_years,
  COUNT(nGDP_USD) AS total_ngdp_records,
  COUNT(rgdp) AS total_rgdp_records,
  COUNT(pop) AS total_pop_records,
  COUNT(cpi) AS total_cpi_records
FROM gmd;
```




At first glance, having data for 245 countries over 945 years seems too good to be true. This issue warrants a double-check to understand it a bit better.




```sql record_per_year
SELECT
  year,
  COUNT(DISTINCT countryname) AS countries_with_data,
  COUNT(*) AS total_records_for_year
FROM
  gmd
GROUP BY
  year
HAVING
  COUNT(DISTINCT countryname) > 30
ORDER BY
  year;
```
From a closer look, we can see that data for more than 30 countries is only available from 1800 onwards. This means that the data before this year is likely to be very sparse and not very useful for our analysis.




Let's take the nGDP_USD variable to understand how nulls are distributed over time and if the database gains completeness as the years go by.








```sql distribution_of_nulls
SELECT
  CASE
      WHEN year >= 1800 AND year < 1850 THEN '1800-1849'
      WHEN year >= 1850 AND year < 1900 THEN '1850-1899'
      WHEN year >= 1900 AND year < 1950 THEN '1900-1949'
      WHEN year >= 1950 AND year < 2000 THEN '1950-1999'
      WHEN year >= 2000 AND year < 2050 THEN '2000-2049'
      ELSE 'Before 1800 or Unknown'
  END AS time_period,
  (SUM(CASE WHEN nGDP_USD IS NULL OR nGDP_USD = 0 THEN 1 ELSE 0 END) * 100.0) / COUNT(*) AS ngdp_null_or_zero_percentage
FROM
  gmd
GROUP BY
  time_period
ORDER BY
  time_period;
```




<AreaChart 
  data={distribution_of_nulls}
  x=time_period
  y=ngdp_null_or_zero_percentage
  title="Percentage of Null or Zero Data per Period"
  yAxisTitle="Percentage (%)"
  xAxisTitle="Period"
/>
We can observe that the volume of missing data has decreased over the years, which is a positive trend. To conclude our foundational analysis and begin delving into indicators that may correlate with money laundering, we will check the data completeness for four key columns


```sql data_quality_check
SELECT 'nGDP_USD' AS variable,
     (SUM(CASE WHEN nGDP_USD IS NULL OR nGDP_USD = 0 THEN 1 ELSE 0 END) * 100.0) / COUNT(*) AS percentage,
     CASE WHEN (SUM(CASE WHEN nGDP_USD IS NULL OR nGDP_USD = 0 THEN 1 ELSE 0 END) * 100.0) / COUNT(*) > 50 THEN 'red' ELSE 'green' END AS color_category
FROM gmd
UNION ALL
SELECT 'cpi' AS variable,
     (SUM(CASE WHEN cpi IS NULL OR cpi = 0 THEN 1 ELSE 0 END) * 100.0) / COUNT(*) AS percentage,
     CASE WHEN (SUM(CASE WHEN cpi IS NULL OR cpi = 0 THEN 1 ELSE 0 END) * 100.0) / COUNT(*) > 50 THEN 'red' ELSE 'green' END AS color_category
FROM gmd
UNION ALL
SELECT 'pop' AS variable,
     (SUM(CASE WHEN pop IS NULL OR pop = 0 THEN 1 ELSE 0 END) * 100.0) / COUNT(*) AS percentage,
     CASE WHEN (SUM(CASE WHEN pop IS NULL OR pop = 0 THEN 1 ELSE 0 END) * 100.0) / COUNT(*) > 50 THEN 'red' ELSE 'green' END AS color_category
FROM gmd
UNION ALL
SELECT 'rgdp' AS variable,
     (SUM(CASE WHEN rgdp IS NULL OR rgdp = 0 THEN 1 ELSE 0 END) * 100.0) / COUNT(*) AS percentage,
     CASE WHEN (SUM(CASE WHEN rgdp IS NULL OR rgdp = 0 THEN 1 ELSE 0 END) * 100.0) / COUNT(*) > 50 THEN 'red' ELSE 'green' END AS color_category
FROM gmd
```




<BarChart
  data={data_quality_check}
  x=variable
  y=percentage
  title="Percentage of Null or Zero Data by Variable"
  xAxisTitle="Variable"
  yAxisTitle="Percentage (%)"
  swapXY=true
  yFmt=percent
/>


In this final observation, we note that composite indicators have the highest number of missing values. We define these as metrics that are not derived from raw data but are instead created by combining, weighing, and calculating multiple variables to measure complex concepts. A good example is the Human Development Index (HDI). This can be attributed to their greater sophistication, which explains why they only began to appear more recently as data collection and analytical methods advanced. With this, we conclude our initial exploration of the GMD database and are ready to proceed to the next step of our analysis.




------
