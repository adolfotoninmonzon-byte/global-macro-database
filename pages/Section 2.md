# 2. A Tale of Two Markets: Tracing the Historical Genesis of Formal and Informal Economies


To contextualize our data analysis, we will trace the concept of money laundering back to its origins in the 1920s. The term itself is famously attributed to organized crime figures who used legal businesses, such as laundromats, to mix illegal profits with legitimate earnings, thereby "washing" the illicit funds. By analyzing macroeconomic variables by decade, we can gain a deeper understanding of this phenomenon's evolution.


Our primary hypothesis is that official and black markets are fundamentally linked. Therefore, we will begin by analyzing the evolution of institutional quality indicators to understand how they have influenced the creation and growth of these dual markets.




```sql institutional_quality_over_time
SELECT
  time_period,
  AVG(CASE WHEN nGDP_USD IS NOT NULL AND nGDP_USD != 0 THEN nGDP_USD END) AS avg_gdp,
  -- Calculates the median (50th percentile) for inflation
  PERCENTILE_CONT(0.5) WITHIN GROUP (ORDER BY cpi) AS median_inflation,
  -- Calculates the median (50th percentile) for government spending
  PERCENTILE_CONT(0.5) WITHIN GROUP (ORDER BY govexp) AS median_government_spending
FROM (
  SELECT
      year,
      nGDP_USD,
      cpi,
      govexp,
      CASE
          WHEN year >= 1920 AND year < 1930 THEN '1920-1929'
          WHEN year >= 1930 AND year < 1940 THEN '1930-1939'
          WHEN year >= 1940 AND year < 1950 THEN '1940-1949'
          WHEN year >= 1950 AND year < 1960 THEN '1950-1959'
          WHEN year >= 1960 AND year < 1970 THEN '1960-1969'
          WHEN year >= 1970 AND year < 1980 THEN '1970-1979'
          WHEN year >= 1980 AND year < 1990 THEN '1980-1989'
          WHEN year >= 1990 AND year < 2000 THEN '1990-1999'
          WHEN year >= 2000 AND year < 2010 THEN '2000-2009'
          WHEN year >= 2010 AND year < 2020 THEN '2010-2019'
          WHEN year >= 2020 THEN '2020-Present'
          ELSE 'Pre-1920'
      END AS time_period
  FROM gmd
  WHERE
      year >= 1920
      AND nGDP_USD IS NOT NULL AND nGDP_USD != 0
      AND cpi IS NOT NULL AND cpi != 0
      AND govexp IS NOT NULL AND govexp != 0
) AS subquery
GROUP BY
  time_period
ORDER BY
  time_period desc;
```




<LineChart
  data={institutional_quality_over_time}
  x=time_period
  y={['avg_gdp', 'median_inflation', 'median_government_spending']}
  title="Median Macroeconomic Indicators by Decade"
  xAxisTitle="Decade"
  yAxisTitle="Median"
  yAxisOptions={{
    domain: [0,100000]
  }}
/>




An analysis of an economy's health requires examining the relationship between government spending, inflation, and GDP. When a significant rise in inflation and government expenditure occurs without a proportional increase in GDP, it increases the susceptibility to illicit financial flows.


This aligns with criminological research, which argues that offenders are rational actors who identify and exploit institutional vulnerabilities. As authors like Gary Becker and the broader rational choice theory school of thought suggest, criminal behavior is a function of analyzing costs versus benefits. In this context, a weakened macroeconomic framework signals a low-risk environment for activities such as money laundering, as the perceived rewards outweigh the potential costs.




```sql gdp_vs_avg_trade_openness
SELECT
  time_period,
  AVG(CASE WHEN nGDP_USD IS NOT NULL AND nGDP_USD != 0 THEN nGDP_USD END) AS avg_gdp,
  AVG(CASE WHEN exports IS NOT NULL AND imports IS NOT NULL THEN exports + imports END) AS avg_trade_openness
FROM (
  SELECT
      year,
      nGDP_USD,
      exports,
      imports,
      CASE
          WHEN year >= 1920 AND year < 1930 THEN '1920-1929'
          WHEN year >= 1930 AND year < 1940 THEN '1930-1939'
          WHEN year >= 1940 AND year < 1950 THEN '1940-1949'
          WHEN year >= 1950 AND year < 1960 THEN '1950-1959'
          WHEN year >= 1960 AND year < 1970 THEN '1960-1969'
          WHEN year >= 1970 AND year < 1980 THEN '1970-1979'
          WHEN year >= 1980 AND year < 1990 THEN '1980-1989'
          WHEN year >= 1990 AND year < 2000 THEN '1990-1999'
          WHEN year >= 2000 AND year < 2010 THEN '2000-2009'
          WHEN year >= 2010 AND year < 2020 THEN '2010-2019'
          WHEN year >= 2020 THEN '2020-Present'
          ELSE 'Pre-1920'
      END AS time_period
  FROM gmd
  WHERE year >= 1920
) AS subquery
GROUP BY
  time_period
ORDER BY
  time_period desc;
```
<AreaChart
  data={gdp_vs_avg_trade_openness}
  x=time_period
  y={['avg_gdp', 'avg_trade_openness']}
  title="Average GDP vs. Trade Openness Over Time"
  xAxisTitle="Década"
  yAxisTitle="Promedio (USD)"
/>


We observe that trade openness has not generated a proportional leap in growth. Despite a significant increase in the exchange of goods and services between countries, this factor has not produced a corresponding rise in global GDP. This is a critical decoupling to our theory.


If the exchange of goods and services is skyrocketing but formal GDP doesn't keep pace, it suggests that a portion of that exchange is occurring outside the official system. This gap creates a clear opportunity for illicit activities that thrive on weak regulations and a lack of oversight. Essentially, a growing trade openness acts as a wider channel for the informal economy.


To better understand this phenomenon, we will now analyze a crucial regional factor by selecting representative countries and comparing their data for indicative insights.


```sql regional_comparison
SELECT
  region,
  AVG(CASE WHEN nGDP_USD IS NOT NULL AND nGDP_USD != 0 THEN nGDP_USD END) AS avg_gdp,
  PERCENTILE_CONT(0.5) WITHIN GROUP (ORDER BY cpi) AS median_inflation
FROM (
  SELECT
      year,
      nGDP_USD,
      cpi,
      CASE
          WHEN countryname IN ('United States', 'Canada', 'Mexico') THEN 'North America'
          WHEN countryname IN ('Argentina', 'Brazil', 'Chile', 'Colombia', 'Venezuela') THEN 'South America'
          WHEN countryname IN ('Germany', 'France', 'United Kingdom', 'Spain', 'Italy') THEN 'Europe'
          WHEN countryname IN ('China', 'Japan', 'India', 'South Korea') THEN 'Asia'
          WHEN countryname IN ('Nigeria', 'South Africa', 'Egypt') THEN 'Africa'
          ELSE 'Other'
      END AS region
  FROM gmd
  WHERE year >= 2010 AND year < 2020
) AS subquery
WHERE
  region != 'Other'
  AND nGDP_USD IS NOT NULL AND nGDP_USD != 0
  AND cpi IS NOT NULL AND cpi != 0
GROUP BY
  region
ORDER BY
  avg_gdp DESC;
```




<ScatterPlot
  data={regional_comparison}
  x=median_inflation
  y=avg_gdp
  size=avg_gdp
  color=region
  series=region
  title="Average GDP vs. Median Inflation by Region (2010-2019)"
  xAxisTitle="Median Inflation (%)"
  yAxisTitle="PAverage GDP (USD)"
/>
The last plot reveals a clear pattern: regions with a higher average GDP tend to have lower median inflation rates. This correlation suggests that sustained economic stability is a key indicator of institutional quality and effective governance. Conversely, regions with a lower GDP often experience higher inflation, signaling an economic instabilityß that creates a fertile environment for illicit financial activities




------
