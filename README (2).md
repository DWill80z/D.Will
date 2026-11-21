# Where Is the Wealth in Broward County?

**Lab 1 · Neighborhood demographic profile · Broward County, Florida**
Tools: Python (geopandas, pandas, matplotlib), QGIS, Census API · Data: ACS 2020–2024 5-year estimates

![Median household income by census tract, Broward County](maps/map1_median_income.png)

## Key findings

- **Broward households earn about 4% less than the US median, yet pay about 35% more in rent and 25% more for homes.**
- **Housing is a stretch even in the middle.** In the median tract, a year of median rent equals 31.2% of median income, past the 30% affordability line, and 64% of renters are cost-burdened.
- **Wealth concentrates in two zones:** the western suburbs (Parkland, Southwest Ranches and Weston have median tract incomes above $150,000), and a narrow coastal strip on the county's eastern edge. That strip covers the beachfront and waterfront neighborhoods between the Atlantic Ocean and the Intracoastal Waterway, roughly east of U.S. 1, made up largely of small coastal towns. Hallandale Beach, at the southern end of the coast, is the exception. Lower incomes form a central band; Lauderhill and Lauderdale Lakes are under $53,000.
- **A city's average can hide its extremes.** On average, Fort Lauderdale looks middle-income: its median tract earns $90,000, well below Weston ($152,286) or Parkland ($168,903). Yet it ties Pembroke Pines for the most wealthy neighborhoods in the county, with 13 of Broward's 83 top-fifth tracts. Fort Lauderdale is large and mixed: affluent neighborhoods along the coast and waterways sit beside lower-income neighborhoods inland, and the average blends them into a misleading middle. Small, uniformly wealthy suburbs like Weston post higher averages but hold fewer top tracts. This is why the analysis maps neighborhoods, not just cities.
- **The finding holds, with one caution.** Twenty-eight tracts (7%) have income estimates too uncertain to rely on. Leaving them out moves the median tract income by only 2.5%, from $76,667 to $78,606, so the overall pattern stands. But the median rises when they're removed, which shows the less certain estimates fall mostly in tracts with lower median incomes. Those are the same communities where accurate data matters most for fair decisions on housing, transit and public services.

## Question

Where is the wealth in Broward, how wide is the gap between incomes and housing costs, and do wealthy and struggling neighborhoods face the same flood risk?

This lab answers the first two parts. Flood risk is the next phase (see *Next steps*).

## Data

| Item | Detail |
| --- | --- |
| Source | U.S. Census Bureau, American Community Survey (ACS) 2020–2024 5-year estimates, via the Census API |
| Geography | 416 populated census tracts, Broward County (FIPS 12011), 2024 cartographic boundaries |
| Population covered | 1,977,129 |
| Variables | Median household income (B19013), median home value (B25077), median gross rent (B25064), households earning $200K+ (B19001), rent burden (B25070), vehicles (B08201), race and ethnicity (B03002) |
| Place names | Census 2024 city boundaries; neighborhood names from OpenStreetMap |
| Projection | Florida State Plane East, EPSG:2881 |

The census does not measure wealth (savings, investments) directly, so this analysis uses three stand-ins: **median household income** (what people earn), **median home value** (what they own) and the **share of households earning $200K+** (where the top earners live).

## Method

1. **Downloaded the data with Python.** `lab01_broward_acs.py` pulls the ACS tables from the Census API, calculates percentages and ratios, joins them to tract boundaries and saves one map-ready GeoPackage. Anyone can rerun it to reproduce the results.
2. **Built two gap measures:** *rent-to-income* (a year of median rent as a % of median income) and *value-to-income* (how many years of median income the median home costs).
3. **Checked reliability.** For each tract I computed the coefficient of variation (CV) of median income. 28 tracts (7%) have a CV above 30% and are flagged as unreliable.
4. **Ran a robustness check:** recalculated the median with and without the unreliable tracts.
5. **Mapped income** in QGIS and Python, using 5 equal-count classes (quantiles), with the unreliable tracts hatched. I chose a purple-red ramp because a light green one hid the tract boundaries.
6. **Added place names with spatial joins.** Each tract was matched to the city boundary it falls inside, and to the nearest OpenStreetMap neighborhood within one mile. Tracts with no neighborhood match (292 of 416) are labeled by city.
7. **Benchmarked against official figures** for Broward County and the US.

The full workflow, with explanations, is in `lab01_broward_wealth.ipynb`.

## Findings

### 1. Where the wealth is

- **The median tract has a median household income of $76,667.** That's within $1,000 of the official countywide median ($77,633), which confirms the tract data lines up with the published figure.
- **The highest-income fifth of tracts sits in the western suburbs and along the coast.** Lower incomes form a band through the central and eastern part of the county, inland from the coast.
- **Incomes are skewed right.** Most tracts sit between about $50,000 and $100,000, peaking just below $80,000, while a long tail of tracts reaches past $200,000. A small number of very high-income tracts sit far above everyone else.

![How Broward's tracts are spread across income levels](maps/chart_income_distribution.png)

### 2. Which cities hold it

![Median tract income by city](maps/chart_income_by_city.png)

![Cities holding Broward's top-fifth income tracts](maps/chart_wealth_by_city.png)

- **The western suburbs lead.** The highest median tract incomes are in **Parkland ($168,903)**, **Southwest Ranches ($165,592)**, **Weston ($152,286)** and **Cooper City ($124,485)**. The first three are about twice the countywide median tract ($76,667).
- **The central band trails.** The lowest are **Lauderhill ($47,621)**, **Hallandale Beach ($49,662)**, **Pembroke Park ($51,188)** and **Lauderdale Lakes ($52,449)**, under two-thirds of the median tract.
- **Big, mixed cities hold much of the top tier.** Of the 83 highest-income tracts, **Fort Lauderdale** and **Pembroke Pines** hold 13 each (16% apiece), followed by **Weston (9)**, **Coral Springs (8)** and **Miramar (7)**. Fort Lauderdale's median tract is only $90,000, mid-pack, yet it ties for the most top-fifth tracts. Its affluent coastal and waterfront neighborhoods sit beside lower-income inland ones, and the city-wide average blends them into a misleading middle.
- **A coastal exception:** **Hallandale Beach** is on the coast but ranks near the bottom on income. That likely reflects a large retiree population: low *incomes* can still come with substantial *assets*, which is a reminder that income is only a stand-in for wealth.

Some cities, such as Parkland (4 tracts) and Southwest Ranches (3), have few tracts, so their medians rest on small numbers. Cities with fewer than 3 tracts are left out of the bar chart.

### 3. How Broward compares with the US

![Broward vs. the US, indexed to US = 100](maps/chart_broward_vs_us.png)

| Measure (ACS 2020–2024) | Broward County | United States | Difference |
| --- | --- | --- | --- |
| Median household income | $77,633 | $80,734 | −4% |
| Median home value | $414,600 | $332,700 | +25% |
| Median monthly gross rent | $1,907 | $1,413 | +35% |
| A year of rent as a % of income | 29.5% | 21.0% | at the 30% affordability line |
| Home value in years of income | 5.3 | 4.1 | rule of thumb is about 3 |

Nationally, nearly half of renter households are cost-burdened. In the median Broward tract, it's **64%**.

### 4. The gap inside Broward (median tract)

| Measure | Value | Benchmark |
| --- | --- | --- |
| Median home value | $402,450 | |
| Median monthly rent | $1,934 | |
| Households earning $200K+ | 8.4% | |
| A year of rent as a % of income | 31.2% | above the 30% line |
| Home value in years of income | 5.0 | about 3 is affordable |
| Renters who are cost-burdened | 64.4% | about 49% nationally |

### 5. Robustness check

![Tract incomes, reliable vs. unreliable estimates](maps/chart_reliability.png)

| | Median tract income |
| --- | --- |
| All 416 tracts | $76,667 |
| 388 reliable tracts only | $78,606 |
| Difference | +$1,939 (+2.5%) |

Leaving out the unreliable tracts barely moves the result, so the pattern holds. But the median moves *up* when they're removed, and it could only rise if most of the removed tracts sat below it. The less certain estimates fall mostly in tracts with lower median incomes, where decisions on housing, transit and public services depend most on accurate data. Results for those tracts should be confirmed with local sources before they guide decisions.

## What this means for planning

Broward's wealth is not in one place, and neither is its risk. The coast and the western suburbs are both high-income, but they face different futures. Coastal neighborhoods sit in flood-prone areas, while the western suburbs are newer development on former wetlands. The inland middle is squeezed by housing costs in a county where even the typical neighborhood is past the affordability line. Housing policy here can't target a few poor pockets: affordability pressure is countywide, and it's sharpest for renters.

## Limitations

- Income and home value are stand-ins for wealth. They don't capture savings, investments or debt.
- ACS income is self-reported, and every figure is a survey estimate with a margin of error.
- 28 tracts (7%) have unreliable income estimates, mostly lower-income ones.
- Tract figures are medians that hide differences within each tract.
- The tract medians are medians *of* medians, not countywide figures.
- Quantile classes give each color the same number of tracts, so the dollar gaps between classes are unequal.
- OpenStreetMap neighborhood names are volunteer-made, and only 124 of 416 tracts matched one.

## Next steps

- Map median home value and the rent-to-income gap.
- Add FEMA flood zones, and compare the flood exposure of the wealthiest and most cost-burdened tracts.

## Files

| File | What it is |
| --- | --- |
| `lab01_broward_acs.py` | Reproducible data download and indicator calculations |
| `lab01_broward_wealth.ipynb` | Step-by-step notebook: robustness check, maps, charts, place names and benchmarks |
| `maps/` | Exported map and chart images |

## Sources

- U.S. Census Bureau, American Community Survey 2020–2024 5-year estimates (Census API; QuickFacts for Broward County and the US)
- U.S. Census Bureau, 2024 cartographic boundary files (census tracts and places)
- National renter cost burden: Congressional Research Service, *Housing Cost Burdens in 2024: In Brief* (R48945)
- Neighborhood names © OpenStreetMap contributors, available under the Open Database License (ODbL)
