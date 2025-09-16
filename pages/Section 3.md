# 3. Analyzing times of crisis.


Our analysis seeks to reveal the geographical interrelationships between sovereign debt, currency, and banking crises.


Understanding crisis hotspots also allows for an understanding of institutional vulnerabilities, which serve as weak points for the entry of economic crime.


```sql crisis_distribution
SELECT
 countryname,
 SUM(SovDebtCrisis) AS "Total sovdebt Crisis",
 SUM(CurrencyCrisis) AS "Total Currency Crisis",
 SUM(BankingCrisis) AS "Total Banking Crisis",
 (SUM("SovDebtCrisis") + SUM("CurrencyCrisis") + SUM("BankingCrisis")) AS "Total Crises",
 CASE
   WHEN countryname IN ('Argentina', 'Brazil', 'Chile', 'Uruguay', 'Peru', 'Paraguay', 'Mexico', 'Bolivia', 'Ecuador', 'Colombia', 'Venezuela', 'Nicaragua') THEN 'Latin America'
   WHEN countryname IN ('Turkey', 'Spain', 'Hungary', 'Iceland') THEN 'Europe'
   WHEN countryname IN ('Australia', 'Japan', 'China') THEN 'Asia/Pacific'
   WHEN countryname IN ('Congo DR') THEN 'Africa'
   ELSE 'Unknown'
 END AS region
FROM gmd
WHERE
 year >= 1920
GROUP BY
 1
ORDER BY 5 DESC
limit 20;
```
<BubbleChart
 data={crisis_distribution}
 title="Total Crises by Country and Region (Top 20)"
 x="Total sovdebt Crisis"
 y="Total Currency Crisis"
 size="Total Crises"
 tooltipTitle="countryname"
 series=region
/>
What We Found


1. **Predominance of Inflation in Latin America**: We observed that Latin American countries tend to be the most affected by inflation crises. This finding is consistent with the data and our initial perception.


2. **Crises Occur Together**: Our analysis reveals that debt crises, currency crises, and banking crises often overlap. This suggests that these crises are not isolated events but are interconnected, much like a domino effect where one crisis can trigger another.


3. **Argentina's Unique Case**: We noticed that Argentina presents a unique case with a balanced distribution of all three types of crises. This suggests that throughout its history, the country has consistently experienced these crises in tandem, rather than being predominantly affected by just one.
