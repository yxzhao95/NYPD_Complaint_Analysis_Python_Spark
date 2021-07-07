# NYPD_Complaint_Analysis_Python_Spark

## Problem Description:
As more people are vaccinated in the US, there has been a significant travel rebound. New York City has always been the top 10 traveling destinations in the US. Its shopping and food selections attract people from all over the country. Safety is perhaps the number one concern in any trip. Therefore, this project aims to explore some criminal patterns in NYC and deliver some insights and suggestions to the visitors using the data from NYPD. In addition, I'm currently located in NYC, so I'd like to know more about the criminal patterns in the city and try to avoid any unpleasant incidents.
## Project Objectives:
  1. The number of complaints for different category
  2. The number of complaints for different district
  3. The number of "" cimre at "Lower and Mid-Town Manhattan"
  4. The number of crime in each month of 2014, 2015, 2016, 2017, 2018
  5. Top danger district and complaint count in each hour
  6. Spark ML clustering for spatial analysis

## Data Source:
https://data.cityofnewyork.us/Public-Safety/NYPD-Complaint-Data-Historic/qgea-i56i

Original Dataset:<br/> 
Rows: 6.91M<br/>Columns: 35

Filtered:<br/>
Rows: 2.37M<br/>Columns: 35

## Data Preprocessing
  ### Make assumptions and drop columns <br/>
  PARKS_NM", "VIC_AGE_GROUP", "VIC_RACE", "VIC_SEX", "STATION_NAME", "PATROL_BORO", "TRANSIT_DISTRICT", 
             "SUSP_SEX", "SUSP_RACE", "X_COORD_CD", "Y_COORD_CD", "HOUSING_PSA", "HADEVELOPT", 
             "JURISDICTION_CODE", "JURIS_DESC", "JURIS_DESC", "RPT_DT", "JURIS_DESC", "PREM_TYP_DESC",
             "PD_CD", "PD_DESC", "LOC_OF_OCCUR_DESC", "CMPLNT_TO_TM", "CMPLNT_TO_DT
  ### Rename the remaining columns<br/>
  'Complaint_ID', 'Complnt_Date','Complnt_Time', 'Neighborhood', 'Offence_Code', 'Offence_Type',
        'Status','Offence_Level','Borough', 'Age','Latitude','Longitude','Lat_Lon'
  ### Get DataFrame and SQL Table
  Create temp view nyc_crime
  
## Analysis
### The number of crimes year over year
![crime_year](https://user-images.githubusercontent.com/72089707/124815037-bf8e4e80-df34-11eb-8ac8-e53aea17f570.png)

### The number of crimes for different category
<img width="350" alt="Screen Shot 2021-07-07 at 2 54 14 PM" src="https://user-images.githubusercontent.com/72089707/124813829-4e9a6700-df33-11eb-97e6-9062e1032850.png">
<img width="700" alt="crime_types" src="https://user-images.githubusercontent.com/72089707/124818750-683ead00-df39-11eb-924d-bf02a5ff60a7.png">

### The number of crimes for different borough
![crime_boro](https://user-images.githubusercontent.com/72089707/124819461-472a8c00-df3a-11eb-8dd3-69e30f9b2186.png)

### The number of "" cimre at "Lower and Mid-Town Manhattan"

### The number of crime in each month of 2014, 2015, 2016, 2017, 2018
![crime_each_month](https://user-images.githubusercontent.com/72089707/124815351-2ad82080-df35-11eb-9b9b-58260a546583.png)

### Top danger boroughs and "ROBBERY" count in each hour
![crime_hour](https://user-images.githubusercontent.com/72089707/124816054-0d578680-df36-11eb-8f83-262cede1fc24.png)

### Spark ML clustering for spatial analysis

## Conclusion
