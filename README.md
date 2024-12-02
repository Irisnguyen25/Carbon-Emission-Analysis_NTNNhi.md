# Carbon-Emission-Analysis_NTNNhi.md
## Introduction
### What is the project about?
This report aims to analyze carbon emissions to examine the carbon footprint across various industries. We aim to identify sectors with the highest levels of emissions by analyzing them across countries and years, as well as to uncover trends.
Carbon emissions play a crucial role in the environment, accounting for over 75% of global emissions and posing a significant environmental challenge. These emissions contribute to the accumulation of greenhouse gases in the atmosphere, leading to climate change, planetary warming, and involvement in various environmental disasters.
Through this analysis, we hope to gain an understanding of the environmental impact of different industries and contribute to making informed decisions in sustainable development.
## Data sources
Our dataset is compiled from publicly available data from nature.com and encompasses the product carbon footprints (PCF) for various companies. PCFs represent the greenhouse gas emissions associated with specific products, quantified in CO2 (carbon dioxide equivalent).
## Data Structure
### Table product_emissions
```sql
select * from product_emissions LIMIT 5;
``` 
| id           | company_id | country_id | industry_group_id | year | product_name                                                    | weight_kg | carbon_footprint_pcf | upstream_percent_total_pcf | operations_percent_total_pcf | downstream_percent_total_pcf | 
| -----------: | ---------: | ---------: | ----------------: | ---: | --------------------------------------------------------------: | --------: | -------------------: | -------------------------: | ---------------------------: | ---------------------------: | 
| 10056-1-2014 | 82         | 28         | 2                 | 2014 | Frosted Flakes(R) Cereal                                        | 0.7485    | 2                    | 57.50                      | 30.00                        | 12.50                        | 
| 10056-1-2015 | 82         | 28         | 15                | 2015 | "Frosted Flakes, 23 oz, produced in Lancaster, PA (one carton)" | 0.7485    | 2                    | 57.50                      | 30.00                        | 12.50                        | 
| 10222-1-2013 | 83         | 28         | 8                 | 2013 | Office Chair                                                    | 20.68     | 73                   | 80.63                      | 17.36                        | 2.01                         | 
| 10261-1-2017 | 14         | 16         | 25                | 2017 | Multifunction Printers                                          | 110       | 1488                 | 30.65                      | 5.51                         | 63.84                        | 
| 10261-2-2017 | 14         | 16         | 25                | 2017 | Multifunction Printers                                          | 110       | 1818                 | 25.08                      | 4.51                         | 70.41                        | 
### Table 'industry_groups'
```sql
select * from industry_groups LIMIT 5;
```
| id | industry_group                                                         | 
| -: | ---------------------------------------------------------------------: | 
| 1  | "Consumer Durables, Household and Personal Products"                   | 
| 2  | "Food, Beverage & Tobacco"                                             | 
| 3  | "Forest and Paper Products - Forestry, Timber, Pulp and Paper, Rubber" | 
| 4  | "Mining - Iron, Aluminum, Other Metals"                                | 
| 5  | "Pharmaceuticals, Biotechnology & Life Sciences"                       | 
### Table 'companies'
``` sql
select * from companies LIMIT 5;
```
| id | company_name                  | 
| -: | ----------------------------: | 
| 1  | "Autodesk, Inc."              | 
| 2  | "Casio Computer Co., Ltd."    | 
| 3  | "Cisco Systems, Inc."         | 
| 4  | "CNX Coal Resources, LP"      | 
| 5  | "Coca-Cola Enterprises, Inc." | 
### Table 'countries'
``` sql
select * from countries LIMIT 5;
```
| id | country_name | 
| -: | -----------: | 
| 1  | Australia    | 
| 2  | Belgium      | 
| 3  | Brazil       | 
| 4  | Canada       | 
| 5  | Chile        | 

## Project brief
### TOP 10 products release the most carbon emissions
```sql
select  
	product_name, industry_group,
	ROUND(avg(carbon_footprint_pcf),2) average_footprint
from product_emissions p
JOIN industry_groups i 
ON p.industry_group_id = i.id
GROUP BY product_name, industry_group_id 
ORDER BY avg(carbon_footprint_pcf) DESC
LIMIT 10;
```
| product_name                                                                                                                       | industry_group                     | average_footprint | 
| ---------------------------------------------------------------------------------------------------------------------------------: | ---------------------------------: | ----------------: | 
| Wind Turbine G128 5 Megawats                                                                                                       | Electrical Equipment and Machinery | 3718044.00        | 
| Wind Turbine G132 5 Megawats                                                                                                       | Electrical Equipment and Machinery | 3276187.00        | 
| Wind Turbine G114 2 Megawats                                                                                                       | Electrical Equipment and Machinery | 1532608.00        | 
| Wind Turbine G90 2 Megawats                                                                                                        | Electrical Equipment and Machinery | 1251625.00        | 
| Land Cruiser Prado. FJ Cruiser. Dyna trucks. Toyoace.IMV def unit.                                                                 | Automobiles & Components           | 191687.00         | 
| Retaining wall structure with a main wall (sheet pile): 136 tonnes of steel sheet piles and 4 tonnes of tierods per 100 meter wall | Materials                          | 167000.00         | 
| TCDE                                                                                                                               | Materials                          | 99075.00          | 
| Mercedes-Benz GLE (GLE 500 4MATIC)                                                                                                 | Automobiles & Components           | 91000.00          | 
| Mercedes-Benz S-Class (S 500)                                                                                                      | Automobiles & Components           | 85000.00          | 
| Mercedes-Benz SL (SL 350)                                                                                                          | Automobiles & Components           | 72000.00          | 
#### Insights:
##### Wind Turbine
- Wind Turbine, Electrical Equipment and Machinery group, is a product line releasing the most carbon emission into the air, including Wind Turbine G128 5 Megawatts (3,718,044), Wind Turbine G132 5 Megawatts, Wind Turbine G114 2 Megawatts, Wind Turbine G90 2 Megawatts. 
→ Wind Turbines are known for their environmentally- friendly benefits in terms of renewable energy generation, but their manufacturing process is highly carbon-intensive due to its components production, transportation or installation. → Renewables are not Fully Green
The higher emission values for larger turbines (G128, G132) align with the scale of material and energy required for manufacturing.
##### Automobiles and Components
- The following products are SUV cars and trucks, automobiles and components industry, have carbon emission driven by their production and assembly. 
Land Cruiser Prado and similar vehicles rank higher, likely due to their focus on durability and off-road capabilities, which demand more materials and larger engines.
Luxury vehicles like Mercedes-Benz models emit less compared to larger trucks but remain significant due to premium materials and performance features.
→ Utility-focused vehicles like Land Cruisers have a higher carbon footprint compared to luxury vehicles, despite both relying on carbon-heavy manufacturing processes → focus on researching hybrid or electric SUV
##### Materials - heavy infrastructure (Retaining Wall Structure and TDCE)
Retaining walls require large quantities of steel and other industrial materials, which are highly carbon-intensive to produce.
The process of manufacturing steel involves significant use of coal in blast furnaces, explaining the high emissions.

### Top 3 industries having the highest level of carbon emissions
``` sql
select  
	industry_group,
	ROUND(avg(carbon_footprint_pcf),2) average_footprint
from product_emissions p
JOIN industry_groups i 
ON p.industry_group_id = i.id
GROUP BY industry_group 
ORDER BY avg(carbon_footprint_pcf) DESC
LIMIT 5;
```
| industry_group                                   | average_footprint | 
| -----------------------------------------------: | ----------------: | 
| Electrical Equipment and Machinery               | 891050.73         | 
| Automobiles & Components                         | 35373.48          |
| "Pharmaceuticals, Biotechnology & Life Sciences" | 24162.00          | 
| Capital Goods                                    | 7391.77           | 
| Materials                                        | 3208.86           | 
#### Insights:
Electrical Equipment and Machinery both has the highest level of carbon emissions releasing into the air (average carbon footprint:891050.73) and include type of product contributing the most carbon dioxide (Wind Turbine G128 5 Megawats)
### TOP 5 companies with the highest contribution to carbon emissions
``` sql
select  
	company_name, industry_group,
	ROUND(avg(carbon_footprint_pcf),2) average_footprint
from product_emissions p
JOIN companies c
ON p.company_id = c.id
JOIN industry_groups i 
ON p.industry_group_id = i.id
GROUP BY company_name, industry_group
ORDER BY avg(carbon_footprint_pcf) DESC
LIMIT 5;
```
| company_name                           | industry_group                     | average_footprint | 
| -------------------------------------: | ---------------------------------: | ----------------: | 
| "Gamesa Corporación Tecnológica, S.A." | Electrical Equipment and Machinery | 2444616.00        | 
| "Hino Motors, Ltd."                    | Automobiles & Components           | 191687.00         | 
| Arcelor Mittal                         | Materials                          | 83503.50          | 
| Weg S/A                                | Capital Goods                      | 70323.50          | 
| Daimler AG                             | Automobiles & Components           | 43089.19          | 
#### Insights:
##### Dominance of Electrical Equipment and Machinery
- Company: Gamesa Corporación Tecnológica, S.A (average_footprint: 2,444,616)
- This company has an overwhelmingly large carbon footprint compared to others on the list. It could indicate energy-intensive manufacturing or reliance on non-renewable energy sources in production.
##### Overall
- All companies on this list are from sectors known for high energy and resource demands (automobiles, materials, machinery, capital goods). Transitioning to renewable energy and adopting circular economy principles could mitigate their environmental impact.
- Carbon Footprint Gaps: The vast difference in the average carbon footprints (e.g., Gamesa vs. Daimler) might indicate differences in company size, operations scope, or energy efficiency measures.

### TOP 5 countries with the highest contribution to carbon emissions
``` sql
SELECT
    Distinct country_name AS Country,
    industry_group AS The_highest_industry_contribution,
    ROUND((AVG(p.carbon_footprint_pcf)), 2) AS Average_Carbon_Emission
FROM
    product_emissions p
JOIN
    countries c ON p.country_id = c.id
JOIN
    industry_groups i ON p.industry_group_id = i.id
GROUP BY
    country_name
ORDER BY
     Average_Carbon_Emission DESC LIMIT 5;
```
| Country     | The_highest_industry_contribution | Average_Carbon_Emission | 
| ----------: | --------------------------------: | ----------------------: | 
| Spain       | Materials                         | 699009.29               | 
| Luxembourg  | Materials                         | 83503.50                | 
| Germany     | Automobiles & Components          | 33600.37                | 
| Brazil      | Materials                         | 9407.61                 | 
| South Korea | Materials                         | 5665.61                 | 
#### Insights
- The "Materials" industry is the highest contributor to carbon emissions in three out of the five countries listed: Spain, Luxembourg, and Brazil.
This indicates that the "Materials" industry (e.g., mining, metals, and construction materials) plays a significant role in global emissions, particularly in these regions.
- Spain has an exceptionally highest average carbon emission of 699,009.29, far exceeding all other countries in this table.
- Despite being a small country, Luxembourg has a notable contribution from the "Materials" industry with an average emission of 83,503.50.
- Germany's "Automobiles & Components" industry ranks as the highest contributor to emissions in the country, with 33,600.37 as the average emission.
This reflects Germany's strong presence in the global automotive market, where manufacturing processes and supply chains for automobiles are carbon-intensive.
##### Trends:
###### Industrialized Nations:
- Countries like Spain, Luxembourg, and Germany showcase how industrialization and consumption patterns drive emissions.
- Even countries with strong renewable energy initiatives (Germany) still face challenges due to heavy industries.
###### Emerging Economies:
- Brazil illustrates the tension between development needs and environmental sustainability. Deforestation and energy demand in emerging economies contribute significantly.
- Luxembourg, despite its size, highlights how wealth and consumption can lead to high emissions per capita.
### TREND of PCFs over years
``` sql
select  
	year, avg(carbon_footprint_pcf) Average_carbon_footprint
FROM product_emissions
GROUP BY year;
```
| year | Average_carbon_footprint | 
| ---: | -----------------------: | 
| 2013 | 2399.3190                | 
| 2014 | 2457.5827                | 
| 2015 | 43188.9044               | 
| 2016 | 6891.5210                | 
| 2017 | 4050.8452                | 
#### Insights: 
##### 2013-2014: Slight decrease
- A small increase from 2399.32 in 2013 to 2457.58 in 2014.
##### 2014-2015: Considerable increase
- The average carbon footprint rises dramatically to 43,188.90 in 2015, a staggering increase of over 1,656% compared to 2014. 
- It maybe due to economic growth, policies changes.
##### 2015-2017: Significant decrease
- In 2016, the average carbon footprint falls sharply to 6891.52, representing an 84% decrease from the peak in 2015 and continue to decrease by 41% in 2017
--> - Success in implementing sustainable practices or energy efficiency measures.
- Transition to renewable energy sources or reduced reliance on fossil fuels.
### TREND of PCFs of industries over years
``` sql
SELECT 
    i.industry_group,
    ROUND(AVG(CASE WHEN p.year = 2013 THEN p.carbon_footprint_pcf ELSE 0 END), 2) AS avg_2013,
    ROUND(AVG(CASE WHEN p.year = 2014 THEN p.carbon_footprint_pcf ELSE 0 END), 2) AS avg_2014,
    ROUND(AVG(CASE WHEN p.year = 2015 THEN p.carbon_footprint_pcf ELSE 0 END), 2) AS avg_2015,
    ROUND(AVG(CASE WHEN p.year = 2016 THEN p.carbon_footprint_pcf ELSE 0 END), 2) AS avg_2016,
    ROUND(AVG(CASE WHEN p.year = 2017 THEN p.carbon_footprint_pcf ELSE 0 END), 2) AS avg_2017
FROM 
    product_emissions p
JOIN 
    industry_groups i
ON 
    p.industry_group_id = i.id
GROUP BY 
    i.industry_group;
```
| industry_group                                                         | avg_2013 | avg_2014 | avg_2015  | avg_2016 | avg_2017 | 
| ---------------------------------------------------------------------: | -------: | -------: | --------: | -------: | -------: | 
| "Consumer Durables, Household and Personal Products"                   | 0.00     | 0.00     | 116.38    | 0.00     | 0.00     | 
| "Food, Beverage & Tobacco"                                             | 39.02    | 20.98    | 0.00      | 783.51   | 24.70    | 
| "Forest and Paper Products - Forestry, Timber, Pulp and Paper, Rubber" | 0.00     | 0.00     | 685.31    | 0.00     | 0.00     | 
| "Mining - Iron, Aluminum, Other Metals"                                | 0.00     | 0.00     | 2727.00   | 0.00     | 0.00     | 
| "Pharmaceuticals, Biotechnology & Life Sciences"                       | 10757.00 | 13405.00 | 0.00      | 0.00     | 0.00     | 
| "Textiles, Apparel, Footwear and Luxury Goods"                         | 0.00     | 0.00     | 14.33     | 0.00     | 0.00     | 
| Automobiles & Components                                               | 1783.41  | 3150.89  | 11194.89  | 19244.29 | 0.00     | 
| Capital Goods                                                          | 1719.71  | 2677.11  | 100.14    | 181.97   | 2712.83  | 
| Chemicals                                                              | 0.00     | 0.00     | 1949.03   | 0.00     | 0.00     | 
| Commercial & Professional Services                                     | 26.30    | 10.84    | 0.00      | 65.68    | 16.84    | 
| Consumer Durables & Apparel                                            | 42.16    | 48.24    | 0.00      | 17.09    | 0.00     | 
| Containers & Packaging                                                 | 0.00     | 0.00     | 373.50    | 0.00     | 0.00     | 
| Electrical Equipment and Machinery                                     | 0.00     | 0.00     | 891050.73 | 0.00     | 0.00     | 
| Energy                                                                 | 150.00   | 0.00     | 0.00      | 2004.80  | 0.00     | 
| Food & Beverage Processing                                             | 0.00     | 0.00     | 7.05      | 0.00     | 0.00     | 
| Food & Staples Retailing                                               | 0.00     | 32.21    | 29.42     | 0.08     | 0.00     | 
| Gas Utilities                                                          | 0.00     | 0.00     | 61.00     | 0.00     | 0.00     | 
| Household & Personal Products                                          | 0.00     | 0.00     | 0.00      | 0.00     | 0.00     | 
| Materials                                                              | 1113.96  | 420.43   | 0.00      | 490.37   | 1184.09  | 
| Media                                                                  | 643.00   | 643.00   | 127.93    | 120.53   | 0.00     | 
| Retailing                                                              | 0.00     | 3.80     | 2.20      | 0.00     | 0.00     | 
| Semiconductors & Semiconductor Equipment                               | 0.00     | 10.00    | 0.00      | 0.80     | 0.00     | 
| Semiconductors & Semiconductors Equipment                              | 0.00     | 0.00     | 1.00      | 0.00     | 0.00     | 
| Software & Services                                                    | 0.18     | 4.29     | 672.24    | 671.94   | 20.29    | 
| Technology Hardware & Equipment                                        | 228.84   | 626.82   | 397.59    | 5.87     | 103.34   | 
| Telecommunication Services                                             | 5.78     | 20.33    | 20.33     | 0.00     | 0.00     | 
| Tires                                                                  | 0.00     | 0.00     | 1011.00   | 0.00     | 0.00     | 
| Tobacco                                                                | 0.00     | 0.00     | 1.00      | 0.00     | 0.00     | 
| Trading Companies & Distributors and Commercial Services & Supplies    | 0.00     | 0.00     | 39.83     | 0.00     | 0.00     | 
| Utilities                                                              | 30.50    | 0.00     | 0.00      | 30.50    | 0.00     | 
#### Insights: 
##### Significant Decreases in PCFs
- Food, Beverage & Tobacco: Decreased from 39.02 (2014) to 24.70 (2017), indicating better sustainability practices in this sector.
- Consumer Durables & Apparel: Dropped from 48.24 (2014) to 17.09 (2016), showing a notable improvement in reducing emissions.
- Technology Hardware & Equipment: Declined from 626.82 (2014) to 103.34 (2017), reflecting advancements in energy efficiency and manufacturing processes.
##### Industries with Fluctuations
- Materials: Started at 1113.96 (2013), dropped significantly in 2014 (420.43), and rose to 1184.09 (2017), suggesting inconsistent sustainability practices.
- Software & Services: Increased drastically from 0.18 (2013) to 672.24 (2015), then decreased to 20.29 (2017), reflecting high variability.
##### Industries with Increasing PCFs
- Automobiles & Components: Increased significantly from 1783.41 (2013) to 19424.29 (2016), possibly due to higher production or inefficient processes.
- Pharmaceuticals, Biotechnology & Life Sciences: Rose steadily from 10757.00 (2013) to 13405.00 (2014), with no data in later years
