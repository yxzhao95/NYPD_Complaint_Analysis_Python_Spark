# NYPD_Complaint_Analysis_Python_Spark

## Problem Description:
As more people are vaccinated in the US, there has been a significant travel rebound. New York City has always been the top 10 traveling destinations in the US. Its shopping and food selections attract people from all over the country. Safety is perhaps the number one concern in any trip. Therefore, this project aims to explore some criminal patterns in NYC and deliver some traveling insights and suggestions to the visitors using the data from NYPD. In addition, I'm currently located in NYC, so I'd like to know more about the criminal patterns in the city and try to avoid any unpleasant incidents.
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
### The number of crimes for different category

### The number of crimes for different district

### The number of "" cimre at "Lower and Mid-Town Manhattan"

### The number of crime in each month of 2014, 2015, 2016, 2017, 2018

### Top danger district and complaint count in each hour

### Spark ML clustering for spatial analysis

## Conclusion
