# Road Traffic Accident Data – United Kingdom

**LINK:** [Kaggle Dataset](https://www.kaggle.com/datasets/atharvasoundankar/road-accidents-dataset?resource=download)

**Group 9:**
* Santos, Christian Lloyd L.
* Basco, Sean Jayphee L.
* Velarde, Arnold B.
* Balarag, Abigail S.

## 📄 Dataset Overview
This dataset contains road accident records from different districts in the UK, such as Southend-on-Sea, Uttlesford, Chelmsford, Brentwood, and Epping Forest. It has around 300,000 accident records that include information about weather, vehicles, road conditions, and accident locations. The dataset can be used for accident analysis, road safety studies, mapping, and traffic research.

---

## 1. General Overview
In this project, we analyzed road accident data using Power BI and data analytics techniques. We aimed to identify accident trends, environmental factors, and road conditions that may affect road safety. We applied data preprocessing, exploratory data analysis, data modeling, and dashboard visualization to present meaningful insights.

## 2.1 Data Collection
The dataset contains around 300,000 road accident records from different districts in the UK, including:
* Southend-on-Sea
* Uttlesford
* Chelmsford
* Brentwood
* Epping Forest

The dataset includes information about:
* Weather conditions
* Vehicle types
* Road surface conditions
* Light conditions
* Accident locations

## 2.2 Data Preprocessing
We cleaned and prepared the dataset using Power Query while following the CLEAN Framework.

**Preprocessing Steps:**

* **Dimension Table Creation:** We extracted unique categories (like Weather, Surface, and Vehicle types) from the raw dataset to create dedicated dimension tables, removing all duplicates in the process to ensure a clean 1-to-1 mapping.

  ![Dimension Table Creation](/Images/Dimention%20Table%20Creation.png)

* **ID Generation:** We created custom alphanumeric index keys (e.g., `Surface_ID`, `Vehicle_ID`, `W-102`) for each dimension table to optimize the database size.
	
  ![Junction](/Images/junction.png)
  ![Light](/Images/light.png)
  ![Road](/Images/road.png)
  ![Surface](/Images/surface.png)
  ![Vehicle](/Images/vehicle.png)
  ![Weather](/Images/weather.png)

* **Table Merging:** We used Left Outer Joins to merge these new IDs back into the central `Fact_Road Accident Data` table.

  ![Table Merging](/Images/merging.png)

* **Redundancy Removal:** We strictly deleted the leftover categorical text columns (like `Road_Surface_Conditions`) from the Fact table. This prevented Power BI from getting confused between text and IDs, ensuring our Matrix visuals calculated correctly.

* **Date & Time Extraction:** We derived new time-series hierarchies from the raw `Accident Date` column. Specifically, we extracted `Month Name`, `Month Number`, `Day_of_Week`, `Clean Day Name`, and `Clean Day Num` to ensure our chronological line charts sorted correctly instead of alphabetically.

  ![Date Extraction](/Images/date.png)

---

## 📖 Data Dictionary
Below is the structural definition of the optimized Star Schema used in this project, outlining the Fact table and key Dimension tables.

### Fact_Road Accident Data 
| Field Name | Data Type | Description | Example |
| :--- | :--- | :--- | :--- |
| `Accident_Index` | String | Primary Key. Unique identifier assigned to each recorded accident | `BS0000139` |
| `Accident Date` | Date | The specific calendar date the accident occurred | `Wednesday, August 4, 2021` |
| `Day_of_Week` | String | The day of the week the accident occurred | `Wednesday` |
| `Junction_Control` | String | Type of control at the junction (e.g., give way, auto traffic signal) | `Give way or uncontrolled` |
| `Accident_Severity` | String | Classification of the severity of the accident | `Slight` |
| `Latitude` | Float | Geographic latitude coordinate of the accident | `51.494936` |
| `Local_Authority_(District)` | String | Geographic district where the accident was registered | `Kensington and Chelsea` |
| `Carriageway_Hazards` | String | Any hazards present on the carriageway during the incident | `None` |
| `Longitude` | Float | Geographic longitude coordinate of the accident | `-0.185032` |
| `Number_of_Casualties` | Integer | Total count of individuals injured or killed in the incident | `1` |
| `Number_of_Vehicles` | Integer | Total count of vehicles involved in the incident | `2` |
| `Police_Force` | String | The police jurisdiction that responded to the accident | `Metropolitan Police` |
| `Speed_limit` | Integer | The legal speed limit of the road where the accident occurred | `30` |
| `Time` | Time | The specific time of day the accident occurred | `6:30:00 PM` |
| `Urban_or_Rural_Area` | String | Categorical distinction of the accident environment | `Urban` |
| `Month Name` | String | Extracted month name for time-series analysis | `Aug` |
| `Month Number` | Integer | Extracted month number for chronological sorting | `8` |
| `Clean Day Num` | Integer | Extracted day number for chronological weekly sorting | `3` |
| `Clean Day Name` | String | Extracted clean day name for reporting | `Wednesday` |
| `Junction_ID` | String | Foreign Key. Links to the Dim_Junction dimension table | `J-109` |
| `Light_ID` | String | Foreign Key. Links to the Dim_Light dimension table | `L-105` |
| `Dim_Road.Road_ID` | String | Foreign Key. Links to the Dim_Road dimension table | `R-104` |
| `Surface_ID` | String | Foreign Key. Links to the Dim_Surface dimension table | `S-101` |
| `Vehicle_ID` | String | Foreign Key. Links to the Dim_Vehicle dimension table | `V-103` |
| `Weather_ID` | String | Foreign Key. Links to the Weather Schema dimension table | `W-102` |
| `Road_Type` | String | Categorical description of the physical road layout | `Single carriageway` |

### Weather Schema (Dimension) 
| Field Name | Data Type | Description | Example |
| :--- | :--- | :--- | :--- |
| `Weather_ID` | String | Primary Key. Unique identifier for weather conditions | `W-101` |
| `Weather_Conditions` | String | Categorical description of the weather at the time of the crash | `Fine no high winds` |

### Dim_Surface (Dimension) 
| Field Name | Data Type | Description | Example |
| :--- | :--- | :--- | :--- |
| `Surface_ID` | String | Primary Key. Unique identifier for road surface conditions | `S-101` |
| `Road_Surface_Conditions` | String | Categorical description of the physical road state | `Dry` |

### Dim_Vehicle (Dimension) 
| Field Name | Data Type | Description | Example |
| :--- | :--- | :--- | :--- |
| `Vehicle_ID` | String | Primary Key. Unique identifier for the type of vehicle | `V-101` |
| `Vehicle_Type` | String | Categorical classification of the vehicle involved | `Car` |

### Dim_Light (Dimension) 
| Field Name | Data Type | Description | Example |
| :--- | :--- | :--- | :--- |
| `Light_ID` | String | Primary Key. Unique identifier for lighting conditions | `L-101` |
| `Light_Conditions` | String | Categorical description of the lighting at the time of the accident | `Daylight` |

### Dim_Road (Dimension) 
| Field Name | Data Type | Description | Example |
| :--- | :--- | :--- | :--- |
| `Road_ID` | String | Primary Key. Unique identifier for the road type | `R-101` |
| `Road_Type` | String | Categorical description of the physical road layout | `Single carriageway` |

### Dim_Junction (Dimension) 
| Field Name | Data Type | Description | Example |
| :--- | :--- | :--- | :--- |
| `Junction_ID` | String | Primary Key. Unique identifier for the junction type | `J-101` |
| `Junction_Detail` | String | Categorical description of the intersection or junction | `Crossroads` |

---

## 2.3 Exploratory Data Analysis (EDA)
We followed the Pyramid Framework by identifying important KPIs and measures before creating visualizations.

**Measures and KPI Formulas**
We utilized Data Analysis Expressions (DAX) to create dynamic measures that automatically update whenever cross-filtering is applied on the dashboard.

```dax
// Calculates the total volume of individual accident records
Total Accidents = COUNTROWS('Fact_Road Accident Data')

// Calculates the sum of all recorded casualties across all accidents
Total Casualties = SUM('Fact_Road Accident Data'[Number_of_Casualties])

// Optional calculation for Urban specific incidents
Urban Accidents = CALCULATE([Total Accidents], 'Fact_Road Accident Data'[Urban_or_Rural_Area] = "Urban")


**Visualizations Used**
* **Line Graphs:**
  * *Total Accidents by Month:* We used this to identify seasonal trends and accident patterns.
* **Bar Charts:**
  * *Total Accidents by Day:* We compared accidents from Monday to Sunday.
  * *Total Accidents by Weather Conditions:* We analyzed how weather affected accidents.
  * *Total Accidents by Light Conditions:* We compared accidents during daylight and darkness.

**Outcome:** The EDA process helped us validate the data model, ensure our DAX formulas were calculating correctly across dimensions, and identify important variables that were later included in the final dashboard.

---

## 2.4 Data Modeling / Analytics
We used a Star Schema to optimize dashboard performance and improve filtering speed.

**Schema Design:**
* **Fact Table:** `Fact_Road Accident Data` – This table contains the main accident records and numerical values.
* **Dimension Tables:** We connected the Fact table to `Dim_Surface`, `Dim_Vehicle`, `Weather Schema`, `Dim_Road`, `Dim_Junction`, and `Dim_Light`. We configured all relationships as active 1-to-Many relationships.

**Analytical Method:** We used Descriptive Analytics to identify accident trends, analyze environmental conditions, and compare different accident variables.
* *Example:* We compared weather conditions with road surface conditions using a Matrix visualization.

![Data Modeling](/Images/modeling.png)

---

## 2.5 Visualization & Dashboard
We developed an interactive Power BI dashboard using the DASH Framework with a focus on clean UI/UX design.

**Dashboard Features:**
* **KPI Cards:** We displayed important KPIs such as 308K Total Accidents and 418K Casualties.
* **Dashboard Layout:** We arranged the visuals into organized sections:
  * *Left Side:* Accident counts by District
  * *Top Right:* Donut chart comparing Urban vs. Rural accidents
  * *Center:* Monthly trends, Time of Day analysis, and Vehicle Type breakdown
  * *Bottom:* Matrix visualization comparing Weather and Road Surface Conditions
* **Interactivity:** We added clickable visuals, dynamic filtering, and cross-filtering between charts.
  * *Example:* When users select “Urban” in the donut chart, all visuals automatically update to display Urban accident data only.

![Dashboard](/Images/dashboard.png)

---

## 2.6 Insights and Recommendations

### Insights
* We found that some months recorded higher accident rates than others. We found that accidents increased during certain months of the year. 
* We observed that urban areas had more accidents compared to rural areas. Busy city areas recorded more accidents because of heavier traffic and more vehicles. 
* We discovered that poor weather and dark lighting conditions increased accident frequency. Rainy weather and dark roads made driving more dangerous, causing more accidents. 
* We noticed that certain vehicle types were more involved in accidents. Certain vehicles appeared more often in accident records compared to others. 
* We identified that road surface conditions affected accidents during bad weather. Wet or slippery roads during bad weather made accidents more likely to happen. 

### Recommendations
* **Improve road safety awareness during high-risk months:** We should remind drivers to be more careful during months when accidents usually happen more often. 
* **Increase lighting in accident-prone areas:** We should add more street lights in dark places where accidents frequently happen to help drivers see better at night. 
* **Strengthen traffic monitoring in urban districts:** We should improve traffic control and monitoring in busy city areas to reduce accidents. 
* **Improve road maintenance during rainy seasons:** We should repair and maintain roads properly during rainy weather to prevent slippery and dangerous roads.
* **Promote safe driving during bad weather conditions:** Drivers should be encouraged to drive slowly and carefully during rain, fog, or other bad weather conditions.