# 4. The Basel AML Index .
The index was designed as a tool for financial institutions, companies, and governments to assess a country's financial crime risk. A higher score on the index indicates a higher risk. For example, in the data you provided, Myanmar, with a score of 8.17, is at the top of the list, representing the highest risk, while a country like the United States, with a score of 4.81, is considered lower risk.


The index is not based on subjective surveys. It's an aggregate score derived from credible, third-party sources like the Financial Action Task Force (FATF), Transparency International, and the World Bank. This makes the index a valuable, data-driven tool for due diligence and for comparing a country's risk profile to global standards.




```sql basel_data_2024
SELECT 'MYANMAR' AS Country, 8.17 AS Overall_score_2024, 1 AS Ranking_2024, 'Asia' AS Region
UNION ALL
SELECT 'HAITI', 7.92, 2, 'Latin America & Caribbean'
UNION ALL
SELECT 'DEMOCRATIC REPUBLIC OF THE CONGO', 7.73, 3, 'Africa'
UNION ALL
SELECT 'CHAD', 7.60, 4, 'Africa'
UNION ALL
SELECT 'VENEZUELA', 7.59, 5, 'Latin America & Caribbean'
UNION ALL
SELECT 'LAO PDR', 7.53, 6, 'Asia'
UNION ALL
SELECT 'CENTRAL AFRICAN REPUBLIC', 7.49, 7, 'Africa'
UNION ALL
SELECT 'GABON', 7.48, 8, 'Africa'
UNION ALL
SELECT 'REPUBLIC OF THE CONGO', 7.28, 9, 'Africa'
UNION ALL
SELECT 'GUINEA-BISSAU', 7.28, 10, 'Africa'
UNION ALL
SELECT 'CHINA', 7.27, 11, 'Asia'
UNION ALL
SELECT 'MOZAMBIQUE', 7.15, 12, 'Africa'
UNION ALL
SELECT 'LIBERIA', 7.11, 13, 'Africa'
UNION ALL
SELECT 'ALGERIA', 6.92, 14, 'Africa'
UNION ALL
SELECT 'VIETNAM', 6.90, 15, 'Asia'
UNION ALL
SELECT 'KENYA', 6.87, 16, 'Africa'
UNION ALL
SELECT 'NIGERIA', 6.85, 17, 'Africa'
UNION ALL
SELECT 'NIGER', 6.83, 18, 'Africa'
UNION ALL
SELECT 'MALI', 6.81, 19, 'Africa'
UNION ALL
SELECT 'MADAGASCAR', 6.76, 20, 'Africa'
UNION ALL
SELECT 'CAMBODIA', 6.75, 21, 'Asia'
UNION ALL
SELECT 'ANGOLA', 6.71, 22, 'Africa'
UNION ALL
SELECT 'TURKMENISTAN', 6.71, 23, 'Asia'
UNION ALL
SELECT 'ESWATINI', 6.69, 24, 'Africa'
UNION ALL
SELECT 'COMOROS', 6.68, 25, 'Africa'
UNION ALL
SELECT 'CAMEROON', 6.67, 26, 'Africa'
UNION ALL
SELECT 'SIERRA LEONE', 6.49, 27, 'Africa'
UNION ALL
SELECT 'BURKINA FASO', 6.48, 28, 'Africa'
UNION ALL
SELECT 'TOGO', 6.48, 29, 'Africa'
UNION ALL
SELECT 'TAJIKISTAN', 6.45, 30, 'Asia'
UNION ALL
SELECT 'BENIN', 6.44, 31, 'Africa'
UNION ALL
SELECT 'GUINEA', 6.44, 32, 'Africa';
```
<Histogram
   data={basel_data_2024}
   x=Overall_score_2024
   xAxisTitle="Overall_score_2024 hist"
   colorPalette={
       [
       '#cf0d06',
       '#eb5752',
       '#e88a87',
       '#fcdad9',
       ]
   }
/>


If we analyze the first 32 cases, we see a left-skewed asymmetry. This is because we're looking at the highest-risk segment and are likely only observing the right side of a normal distribution. These 32 countries are at high risk of AML.


Now, does this align with our findings regarding crises, institutional strength, inflation, GDP, etc.?


```sql basel_grouped_data
SELECT
 Region,
 COUNT(Country) AS NumberOfCountries
FROM (
 SELECT 'MYANMAR' AS Country, 8.17 AS Overall_score_2024, 1 AS Ranking_2024, 'Asia' AS Region
 UNION ALL
 SELECT 'HAITI', 7.92, 2, 'Latin America & Caribbean'
 UNION ALL
 SELECT 'DEMOCRATIC REPUBLIC OF THE CONGO', 7.73, 3, 'Africa'
 UNION ALL
 SELECT 'CHAD', 7.60, 4, 'Africa'
 UNION ALL
 SELECT 'VENEZUELA', 7.59, 5, 'Latin America & Caribbean'
 UNION ALL
 SELECT 'LAO PDR', 7.53, 6, 'Asia'
 UNION ALL
 SELECT 'CENTRAL AFRICAN REPUBLIC', 7.49, 7, 'Africa'
 UNION ALL
 SELECT 'GABON', 7.48, 8, 'Africa'
 UNION ALL
 SELECT 'REPUBLIC OF THE CONGO', 7.28, 9, 'Africa'
 UNION ALL
 SELECT 'GUINEA-BISSAU', 7.28, 10, 'Africa'
 UNION ALL
 SELECT 'CHINA', 7.27, 11, 'Asia'
 UNION ALL
 SELECT 'MOZAMBIQUE', 7.15, 12, 'Africa'
 UNION ALL
 SELECT 'LIBERIA', 7.11, 13, 'Africa'
 UNION ALL
 SELECT 'ALGERIA', 6.92, 14, 'Africa'
 UNION ALL
 SELECT 'VIETNAM', 6.90, 15, 'Asia'
 UNION ALL
 SELECT 'KENYA', 6.87, 16, 'Africa'
 UNION ALL
 SELECT 'NIGERIA', 6.85, 17, 'Africa'
 UNION ALL
 SELECT 'NIGER', 6.83, 18, 'Africa'
 UNION ALL
 SELECT 'MALI', 6.81, 19, 'Africa'
 UNION ALL
 SELECT 'MADAGASCAR', 6.76, 20, 'Africa'
 UNION ALL
 SELECT 'CAMBODIA', 6.75, 21, 'Asia'
 UNION ALL
 SELECT 'ANGOLA', 6.71, 22, 'Africa'
 UNION ALL
 SELECT 'TURKMENISTAN', 6.71, 23, 'Asia'
 UNION ALL
 SELECT 'ESWATINI', 6.69, 24, 'Africa'
 UNION ALL
 SELECT 'COMOROS', 6.68, 25, 'Africa'
 UNION ALL
 SELECT 'CAMEROON', 6.67, 26, 'Africa'
 UNION ALL
 SELECT 'SIERRA LEONE', 6.49, 27, 'Africa'
 UNION ALL
 SELECT 'BURKINA FASO', 6.48, 28, 'Africa'
 UNION ALL
 SELECT 'TOGO', 6.48, 29, 'Africa'
 UNION ALL
 SELECT 'TAJIKISTAN', 6.45, 30, 'Asia'
 UNION ALL
 SELECT 'BENIN', 6.44, 31, 'Africa'
 UNION ALL
 SELECT 'GUINEA', 6.44, 32, 'Africa'
) AS subquery
GROUP BY
 Region
ORDER BY
 NumberOfCountries DESC;
```
<BarChart
   data={basel_grouped_data}
   x=Region
   y=NumberOfCountries
   title="Basel AML Risks TOP 30 by Region"
/>


It seems there's no direct correlation between our crisis indicator and the data presented. If there were, we would expect to see many more Latin American and Caribbean countries on the chart. Instead, only two Latin American countries are in the top 30, and only Venezuela appeared in the previous analysis.
