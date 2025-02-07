# Arlington_Energy_Analysis

Energy-efficient buildings are essential for creating a sustainable and cost-effective city. An optimized energy system lowers costs, reduces environmental impact, enhances building performance, and ensures equitable access to energy efficiency. By analyzing building energy consumption data in Arlington, VA, I aimed to identify trends, detect anomalies, and assess efficiency across different building types using SQL.

One critical aspect of this analysis was evaluating the impact of Arlington’s award-winning Community Energy Plan (CEP). This long-term vision aims to transform energy generation, usage, and distribution, guiding the county toward a carbon-neutral future by 2050.



📊 What I Analyzed
For this analysis, I was particularly interested in exploring the following areas:

✔️ Electricity & Gas Usage Intensity (EUI) – Energy consumed per square foot. 

✔️ Total & Average Energy Consumption – Understanding consumption patterns across buildings. 

✔️ Identifying Most & Least Energy-Intensive Buildings – Finding inefficiencies. 

✔️ Energy Consumption by Building Type – Which facility types consume the most? 

✔️ Detecting Anomalies in Energy Usage – Spotting outliers and inefficiencies.

Let’s take a look at the data:

This data was retrieved from Data.gov and can be found here. The dataset represents eight-teen years (2000-2018) of building energy usage within Arlington County, VA. The data includes attributes such as Use Type, Address, Year, Electric Per KWH, Natural Gas Per Therm*, among others.

*Therms measure the energy output of a unit of gas. One therm is the amount of energy or heat equivalent to 100,000 BTU, or British Thermal Units.



⚡ Electricity and Gas Intensity
I was interested in analyzing the electricity and gas intensity for different buildings. To achieve this, I used SUM and AVG aggregation functions to calculate total energy consumption and usage per square foot. I used NULLIF, ensuring that I avoid division errors when a building has incomplete data.


Electric Intensity Query

Electric Intensity Result
Electricity Usage Intensity (Highest per Sq Ft) 

🔌🏢 Network Operation Center – High-power computing and server loads.

🚍 Bus Operations (Shirlington & ART Bus Facility) – Transit and lighting needs.

⛸️ Powhatan Skate Park – Extensive lighting & HVAC usage.

Next, the gas usage intensity. I utilized GROUP BY to group the results by building name and SORT BY to sorted them in descending order based on gas usage per square foot. This allowed me to identify the most gas-intensive buildings and compare energy efficiency across different facilities.


Gas Intensity Query



Gas Intensity Result


Gas Usage Intensity (Highest Therm per Sq Ft) 

🔥🏟️ Gunston Bubble (Tented Sports Facility) – Heating for enclosed structure.

🚌 ART Bus Facility – Gas consumption for heating & maintenance.

🔧 DPW Bays – Industrial heating demands.



🏛️ Who Consumes the Most Energy Overall?
Next, I was interested in analyzing the Total & Average Energy Consumption. Once again utilizing aggregations functions like SUM and AVG and using AS statement to label the new data aggregated. I ensured I analyzed the data by place name by using the GROUP BY statement.




Electric Total and Average Query



Electric Total and Average Result
The Water Pollution Plant & Court System (including Police Operations) topped electricity usage—understandable given: 

✔️ Water treatment requires continuous filtration & pumping. 

✔️ Government & law enforcement facilities operate 24/7.


Gas Total and Average Query
```sql
USE SqlProjects
	SELECT
	Place_Name,
	SUM(Electric_KWH) AS Total_Electricity_Usage,
	AVG(Electric_KWH) AS Average_Electricity_Usage,
	SUM(Natural_Gas_Therm) AS Total_Gas_Usage,
	AVG(Natural_Gas_Therm) AS Average_Gas_Usage
FROM Arlington_Energy
GROUP BY Place_Name
ORDER BY Average_Gas_Usage DESC;
```

Gas Total and Average Result
For gas consumption, the Court System, Water Pollution Plant, and Equipment Division led usage—likely due to heating demands in large facilities.



🏢 Energy Consumption by Building Type
To analyze energy consumption across different facility types, I used SQL’s GROUP BY function to categorize building's use type based on their electricity and gas usage.


Electric By Use Type Query



Electric By Use Type Result
Electricity Consumption (Highest Average Usage) 

🔌 🏥 Human Services – Despite having only 5 buildings, this category had the highest average electricity usage. 

🏗️ Specialty Buildings & General Offices followed closely behind.


Gas By Use Type Query
```sql
USE SqlProjects
	SELECT 
	Use_Type,
	COUNT (DISTINCT Place_Name) AS Num_Buildings,
	AVG(Electric_KWH) AS Average_Electric_Usage,
	AVG(Natural_Gas_Therm) AS Average_Gas_Usage
FROM Arlington_Energy
GROUP BY Use_Type
ORDER BY Average_Gas_Usage DESC;
```

Gas By Use Type Results
Gas Consumption (Highest Average Usage) 

🔥 🚔 Public Safety Facilities – With 19 buildings, these had the highest gas usage, likely due to heating needs in police stations, fire departments, and emergency response centers. 

🏢 Specialty Buildings & General Offices ranked next in gas consumption.



🏗️ Are Newer Buildings More Efficient?
I used the CASE WHEN statement to classify buildings into 3 age groups, then used the GROUP BY and AVG function to evaluate how the average amount of energy used based on the age of the building.




Building Group Query
```sql
USE SqlProjects
SELECT 
	AVG(Electric_KWH) AS Avg_Electric_KWH,
	AVG(Natural_Gas_Therm) AS Avg_Gas_Therm,
	CASE WHEN "Year" < 2006 THEN 'Old'
	WHEN "Year" BETWEEN 2006 AND 2012 THEN 'Recent'
	ELSE 'New'
	END AS Building_Age_Group
FROM Arlington_Energy
GROUP BY 
		(CASE WHEN "Year" < 2006 THEN 'Old'
	WHEN "Year" BETWEEN 2006 AND 2012 THEN 'Recent'
	ELSE 'New'
	END)
ORDER BY Avg_Gas_Therm ASC;
```



Building Group Results
🏠 Buildings from 2012+ had the lowest average energy use—indicating better efficiency designs. 

⚡ Buildings from 2006-2012 had the highest electricity consumption—potentially due to increased electronic systems without strict efficiency standards. 

🔥 The oldest buildings (2000-2006) had the highest gas usage—suggesting older heating systems & insulation inefficiencies.



🚨 Detecting Energy Usage Anomalies (Outliers in Consumption)
Next, I wanted to identify buildings with unusually high electricity consumption compared to their historical usage. To achieve this, I used SQL with statistical measures such as average (AVG) and standard deviation (STDEV) to flag outliers.




Electric Outlier Query

```sql
USE SqlProjects
	SELECT
	Place_Name,
	MAX(Year) AS Year,
	SUM(Electric_KWH)AS Total_Electricity_Usage_KWH,
	(AVG(Electric_KWH) + 2 * STDEV(Electric_KWH)) AS Electric_Usage_Outliers
FROM Arlington_Energy
WHERE Electric_KWH > (SELECT AVG(Electric_KWH) + 2 * STDEV(Electric_KWH) FROM Arlington_Energy)
GROUP BY Place_name
ORDER BY Year DESC;
```

Electric Outlier Result

Electricity Outliers 

⚡ 📍 Courthouse Plaza & Water Pollution Control Plant saw a significant surge in electricity usage in 2015. This could be due to operational expansions, equipment upgrades, or inefficiencies in the system.


Gas Outlier Query
Gas Outlier Result

Gas Outliers 

🔥 🏢 Detention Center (2018), Water Pollution Control Plant (2015), and Courts Police (2014) showed unexpected spikes in gas consumption. These anomalies could indicate changes in heating demands, infrastructure issues, or operational shifts.



Key Takeaways
✔️ Critical infrastructure (transit, law enforcement, and water treatment) consumes the most energy—highlighting opportunities for optimization. 

✔️ Older buildings tend to be less energy efficient, reinforcing the importance of retrofitting and modernizing facilities. 

✔️ Facility type impacts energy demand—server-heavy, industrial, and 24/7 operations significantly drive energy intensity. 

✔️Detecting outliers is crucial for identifying inefficiencies, optimizing energy usage, and reducing costs across facilities.

As I continue refining my analysis, please feel free to leave a comment below or connect with me here.
