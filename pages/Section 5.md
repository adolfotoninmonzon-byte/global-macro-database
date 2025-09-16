# 5. Re-evaluating Our Perceptions


For this last section, we will share some valuable insights that caught our attention and completely turned our initial beliefs upside down.


```sql final_join
SELECT
 t1.Country,
 t1.Region,
 t1.ISO3,
 t2.year,
 AVG(t2.govexp) AS "Avg Government Spending",
 AVG(t2.infl) AS "Avg Inflation",
 AVG(t2.nGDP_USD) AS "Avg GDP"
FROM (
 SELECT 'MYANMAR' AS Country, 'Asia' AS Region, 'MMR' AS ISO3
 UNION ALL
 SELECT 'HAITI', 'Latin America & Caribbean', 'HTI'
 UNION ALL
 SELECT 'DEMOCRATIC REPUBLIC OF THE CONGO', 'Africa', 'COD'
 UNION ALL
 SELECT 'CHAD', 'Africa', 'TCD'
 UNION ALL
 SELECT 'VENEZUELA', 'Latin America & Caribbean', 'VEN'
 UNION ALL
 SELECT 'LAO PDR', 'Asia', 'LAO'
 UNION ALL
 SELECT 'CENTRAL AFRICAN REPUBLIC', 'Africa', 'CAF'
 UNION ALL
 SELECT 'GABON', 'Africa', 'GAB'
 UNION ALL
 SELECT 'REPUBLIC OF THE CONGO', 'Africa', 'COG'
 UNION ALL
 SELECT 'GUINEA-BISSAU', 'Africa', 'GNB'
 UNION ALL
 SELECT 'CHINA', 'Asia', 'CHN'
 UNION ALL
 SELECT 'MOZAMBIQUE', 'Africa', 'MOZ'
 UNION ALL
 SELECT 'LIBERIA', 'Africa', 'LBR'
 UNION ALL
 SELECT 'ALGERIA', 'Africa', 'DZA'
 UNION ALL
 SELECT 'VIETNAM', 'Asia', 'VNM'
 UNION ALL
 SELECT 'KENYA', 'Africa', 'KEN'
 UNION ALL
 SELECT 'NIGERIA', 'Africa', 'NGA'
 UNION ALL
 SELECT 'NIGER', 'Africa', 'NER'
 UNION ALL
 SELECT 'MALI', 'Africa', 'MLI'
 UNION ALL
 SELECT 'MADAGASCAR', 'Africa', 'MDG'
 UNION ALL
 SELECT 'CAMBODIA', 'Asia', 'KHM'
 UNION ALL
 SELECT 'ANGOLA', 'Africa', 'AGO'
 UNION ALL
 SELECT 'TURKMENISTAN', 'Asia', 'TKM'
 UNION ALL
 SELECT 'ESWATINI', 'Africa', 'SWZ'
 UNION ALL
 SELECT 'COMOROS', 'Africa', 'COM'
 UNION ALL
 SELECT 'CAMEROON', 'Africa', 'CMR'
 UNION ALL
 SELECT 'SIERRA LEONE', 'Africa', 'SLE'
 UNION ALL
 SELECT 'BURKINA FASO', 'Africa', 'BFA'
 UNION ALL
 SELECT 'TOGO', 'Africa', 'TGO'
 UNION ALL
 SELECT 'TAJIKISTAN', 'Asia', 'TJK'
 UNION ALL
 SELECT 'BENIN', 'Africa', 'BEN'
 UNION ALL
 SELECT 'GUINEA', 'Africa', 'GIN'
) AS t1
INNER JOIN gmd AS t2
 ON t1.ISO3 = t2.ISO3
WHERE
 t2.year = '2024'
GROUP BY
 t1.Country,
 t1.Region,
 t1.ISO3,
 t2.year
ORDER BY
 t1.Region,
 t2.year;
 ```




<BubbleChart
   data={final_join}
   x="Avg Inflation"
   y="Avg Government Spending"
   series="Region"
   tooltipTitle="Country"
   size="Avg GDP"
   scaleTo=3
   colorPalette={
       [
       '#cf0d06',
       '#eb5752',
       '#e88a87',
       '#fcdad9',
       ]
   }
/>


1. One of our most significant findings was that inflation rates are not the powerful, deterministic indicator of AML risk that we initially believed. This surprising outcome forced us to re-evaluate our foundational assumptions about how macroeconomic factors influence financial crime.
2. Similarly, we found that the disconnect between government spending and GDP is not the strong indicator we might have expected. This discrepancy between a state's finances and its economic output does not have a deterministic correlation with the risk of financial crime.
3. While the presence of macroeconomic crises is not a consistently relevant indicator of risk across the board, it has had a uniquely significant impact on Latin America, making it a crucial risk factor for the region.
4. A significant geographical concentration of risk: Africa accounts for 72% of the top 30 countries in the 2024 Basel AML Index, while Latin America has only two. This suggests that while macroeconomic crises are a factor, other variables—such as corruption, institutional fragility, and the rule of law—may be more significant drivers of a country's overall risk profile. This finding reinforces our previous conclusion that the link between financial instability and illicit activity is not as straightforward as once believed.
5. While our analysis showed that inflation is not a deterministic factor for AML risk, we found that both low government spending and low GDP proved to be effective indicators of this risk.


Notable exceptions to this trend were Vietnam, in terms of its public spending, and China, in terms of its GDP.


**Final words:** The tool used as a 'target' to contrast historical macroeconomic variables was the Basel AML Index. While this tool assesses the risk of money laundering, it is not exempt from interests or biases, just like any other tool. Far from making value judgments, this paper aims to understand and improve the work of compliance analysts, who are increasingly engaging with the world of data to improve their everyday jobs.
