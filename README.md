# 🏙️ Arlington Energy Analysis (2000–2018)

Energy-efficient buildings are essential for creating sustainable, cost-effective cities. An optimized energy system lowers costs, reduces environmental impact, enhances building performance, and ensures equitable access to energy efficiency.  

This SQL-based project analyzes building energy consumption data in **Arlington, VA (2000–2018)** to uncover trends, detect anomalies, and assess efficiency across different building types.  

One critical focus is evaluating the impact of **Arlington’s Community Energy Plan (CEP)**, a long-term vision guiding the county toward carbon neutrality by 2050.  

---

## 📦 Dataset Overview
- Source: [Data.gov – Arlington Building Energy Dataset](https://www.data.gov/)  
- Scope: 18 years of building energy usage  
- Key fields:  
  - `Use_Type` → Facility category (schools, public safety, transit, etc.)  
  - `Year` → Reporting year  
  - `Electric_KWH` → Electricity usage (kWh)  
  - `Natural_Gas_Therm` → Gas usage (therms)  
  - `Place_Name` → Building name  

---

## 🎯 Analysis Objectives
✔️ Electricity & Gas Usage Intensity (EUI) – Energy consumed per square foot  
✔️ Total & Average Energy Consumption – Overall usage across facilities  
✔️ Energy Consumption by Building Type – Which categories are most intensive?  
✔️ Age of Buildings vs. Efficiency – Do newer buildings use less energy?  
✔️ Detecting Outliers – Identifying anomalous consumption  

---

## ⚡ Electricity & Gas Intensity
To compare intensity across facilities, I used `SUM` and `AVG` functions with safeguards (`NULLIF`) to avoid divide-by-zero errors.

### Findings:
- **Electricity Usage Intensity (Highest):**  
  - 🏢 Network Operations Center → heavy server loads  
  - 🚍 Bus Operations Facilities → transit & lighting needs  
  - ⛸️ Powhatan Skate Park → HVAC & lighting-intensive  

- **Gas Usage Intensity (Highest):**  
  - 🏟️ Gunston Bubble → enclosed sports heating  
  - 🚌 ART Bus Facility → industrial heating demands  
  - 🔧 DPW Bays → maintenance & heating load  

---

## 🏛️ Total & Average Energy Consumption
Aggregated totals per building (`SUM`, `AVG`, `GROUP BY Place_Name`).

### Findings:
- **Highest Electricity Consumers:**  
  - 💧 Water Pollution Plant (24/7 operations)  
  - 🚓 Courts & Police Operations (continuous usage)  

- **Highest Gas Consumers:**  
  - 🚨 Court System & Detention Centers → heavy heating demand  
  - 💧 Water Pollution Plant  

---

## 🏢 Consumption by Building Type
Grouped facilities by `Use_Type` to assess categories.

### Findings:
- **Electricity (Highest Average):**  
  - 🏥 Human Services buildings (despite only 5 facilities)  
  - 🏗️ Specialty Buildings & Offices  

- **Gas (Highest Average):**  
  - 🚔 Public Safety Facilities (police, fire, EMS)  
  - 🏢 General Offices  

---

## 🕰️ Efficiency by Building Age
Used `CASE WHEN` to classify buildings into **Old (<2006)**, **Recent (2006–2012)**, and **New (2012+)**.

### Findings:
- 🏠 **2012+ buildings** → lowest average energy usage → modern efficiency standards  
- ⚡ **2006–2012 buildings** → highest electricity consumption → surge in electronics pre-efficiency codes  
- 🔥 **Pre-2006 buildings** → highest gas usage → older heating systems & insulation inefficiencies  

---

## 🚨 Detecting Anomalies
Applied `AVG` + `STDEV` to flag unusually high electricity/gas consumption.

### Findings:
- ⚡ **Electricity Outliers:** Courthouse Plaza & Water Pollution Plant (2015) → potential expansions/inefficiencies  
- 🔥 **Gas Outliers:** Detention Center (2018), Water Pollution Plant (2015), Courts Police (2014) → infrastructure/heating spikes  

---

## 🔑 Key Takeaways
✔️ **Critical infrastructure** (transit, law enforcement, water treatment) drives the most energy demand  
✔️ **Older facilities** are less efficient → retrofitting is key  
✔️ **Facility type matters** → industrial & 24/7 ops consume disproportionately  
✔️ **Outlier detection** highlights inefficiencies & optimization opportunities  

---

## 📊 Project Impact
This analysis demonstrates how **SQL-driven energy analytics** can help:  
- Policymakers prioritize retrofits  
- Facility managers detect inefficiencies early  
- Arlington advance its Community Energy Plan goals toward carbon neutrality  

---


---

👉 Full SQL queries, outputs, and dashboards are included in this repo. Connect with me on [LinkedIn](https://www.linkedin.com/in/isaiah-l-wright/) to discuss SQL-driven energy analytics.  
