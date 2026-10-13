# London Bike-Share Seasonality Analysis

# Objective



Investigate why average hourly bike-share ridership (`cnt`) differs between spring and autumn in Transport for London's bike-share data (2015–2017), using exploratory data analysis to test weather- and calendar-based explanations.

## 

## Dataset

* Source: TfL Cycle Hire data, via the "London bike sharing dataset" on Kaggle (hmavrodiev)
* 17,414 hourly records, January 2015 – January 2017
* Columns: `timestamp`, `cnt`, `t1` (actual temp, °C), `t2` (feels-like temp, °C), `hum` (humidity, %), `wind\_speed` (km/h), `weather\_code`, `is\_holiday`, `is\_weekend`, `season`

## 

## Method

1. **Data integrity check** — no missing values across any column; `hum` capped sensibly at 100; `is\_holiday`/`is\_weekend` correctly binary (0/1).
2. **Verified `season` coding** by cross-referencing the date range of each code against `timestamp` rather than assuming the numeric order (0 = spring, 1 = summer, 2 = autumn, 3 = winter).
3. **Identified the gap worth explaining**: mean `cnt` of 1104 in spring vs 1179 in autumn — roughly a 75-ride difference per hour.
4. **Tested candidate explanations in turn:**



|Factor|Result|Verdict|
|-|-|-|
|Temperature (`t1`, `t2`)|Mean gap of 2–3°C, but std (\~4–5°C) exceeds the gap, the two seasons' temperature distributions overlap heavily|Weak explanation|
|Weather condition|Built a binary "pickup willingness" flag (rideable vs. not) from `weather\_code`, since no authoritative cycling-specific weather index exists for this purpose. Spring: 99.70% "willing" hours vs. autumn: 99.74% — effectively identical|No explanatory power|
|Holidays|Spring: 4.37% of hours are holidays vs. autumn: 0%. Mean `cnt` on holidays (894) is \~253 lower than non-holidays (1147). Weighted contribution ≈ 11 rides|Real effect, but explains only 15% of the gap|
|Wind speed|Correlation with `cnt` = +0.116, weak, and *positive*. Spring is windier yet has lower `cnt`, so wind moves in the wrong direction to explain the gap|Ruled out|

## 

## Findings

The spring–autumn ridership gap is not well explained by temperature, general weather conditions, or wind speed. Lower holiday incidence in spring accounts for a modest share (\~15%) of the gap. The remaining \~85% is likely driven by factors outside this dataset — network growth across the two-year span, marketing activity, or gradual adoption trends — which would need additional data (e.g. date-level trend decomposition) to investigate further.



!\[Average bike-share ridership by season](season\_cnt.png)



## Limitations

* The "pickup willingness" flag is a self-defined, subjective binary classification, not drawn from an established cycling-weather index — flagged explicitly rather than presented as objective.
* Spring/autumn are pooled across two different years without separating year-on-year growth from seasonal effect.
* Comparisons rely on descriptive statistics and correlation; no formal significance testing (t-test, ANOVA) was applied.

## 

## Tools

Python, pandas

## 

## Possible next steps

* Decompose the time series into trend and seasonal components to isolate genuine seasonality from network growth over 2015–2017.
* Formal hypothesis testing (t-test) on the spring vs. autumn `cnt` means.
* Investigate `is\_weekend` and hour-of-day interactions with season.

