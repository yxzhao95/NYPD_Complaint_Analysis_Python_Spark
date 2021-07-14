# NYPD_Complaint_Analysis_Python_Spark

## Problem Description:
As more people are vaccinated in the US, there has been a significant travel rebound. New York City has always been the top 10 traveling destinations in the US. Its shopping and food selections attract people from all over the country. Safety is perhaps the number one concern in any trip. Therefore, this project aims to explore some criminal patterns in NYC and deliver some insights and suggestions to the visitors using the data from NYPD. In addition, I'm currently located in NYC, so I'd like to know more about the criminal patterns in the city and try to avoid any unpleasant incidents.
## Project Objectives:
  1. Analyze complaints by category
  2. Analyze complaints by district
  3. Analyze complaints by month
  4. Analyze complaints by hour
  5. K-Means clustering for spatial analysis

## Data Source:
https://data.cityofnewyork.us/Public-Safety/NYPD-Complaint-Data-Historic/qgea-i56i

Original Dataset:<br/> 
Rows: 6.91M<br/>Columns: 35

Filtered:<br/>
Rows: 2.37M<br/>Columns: 35

## Data Preprocessing
  ### Make assumptions and drop columns <br/>
  PARKS_NM", "VIC_AGE_GROUP", "VIC_RACE", "VIC_SEX", "STATION_NAME", "PATROL_BORO", "TRANSIT_DISTRICT", 
             "SUSP_AGE_GROUP", "SUSP_SEX", "SUSP_RACE", "X_COORD_CD", "Y_COORD_CD", "HOUSING_PSA", "HADEVELOPT", 
             "JURISDICTION_CODE", "JURIS_DESC", "JURIS_DESC", "RPT_DT", "JURIS_DESC", "PREM_TYP_DESC",
             "PD_CD", "PD_DESC", "LOC_OF_OCCUR_DESC", "CMPLNT_TO_TM", "CMPLNT_TO_DT
  ### Rename the remaining columns<br/>
  'Complaint_ID', 'Complnt_Date','Complnt_Time', 'Neighborhood', 'Offence_Code', 'Offence_Type',
        'Status','Offence_Level','Borough', 'Latitude','Longitude','Lat_Lon'
  
## Analysis
### The Number of Crimes Year over Year
```
crime_year = spark.sql("SELECT  SUBSTRING(complnt_date, 7, 4) year, COUNT(*) AS Count 
                        FROM nyc_crime 
                        GROUP BY year 
                        ORDER BY Count DESC")
```
![crime_year](https://user-images.githubusercontent.com/72089707/124815037-bf8e4e80-df34-11eb-8ac8-e53aea17f570.png)

### The Number of Crimes for Different Category
```
crime_types = spark.sql("SELECT  Offence_Type, COUNT(*) AS Count 
                         FROM nyc_crime 
                         GROUP BY Offence_Type 
                         ORDER BY Count DESC")
```
<p float="left">
<img width="660" alt="crime_types" src="https://user-images.githubusercontent.com/72089707/124818750-683ead00-df39-11eb-924d-bf02a5ff60a7.png">
</p>

### The Number of Crimes for Different Borough
```
crime_boro = spark.sql("SELECT  Borough, COUNT(*) AS Count 
                        FROM nyc_crime 
                        GROUP BY Borough 
                        HAVING Borough in ('BROOKLYN', 'MANHATTAN','BRONX','QUEENS','STATEN ISLAND') 
                        ORDER BY Count DESC")
```
![crime_boro](https://user-images.githubusercontent.com/72089707/124819461-472a8c00-df3a-11eb-8dd3-69e30f9b2186.png)

### The Number of Crimes in Each Month of 2014, 2015, 2016, 2017, 2018
```
crime_each_month = spark.sql("SELECT SUBSTRING(Complnt_Date,7,4) AS Year, SUBSTRING(Complnt_Date,0,2) AS Month, COUNT(*) AS Count 
                              FROM nyc_crime
                              GROUP BY Year, Month 
                              HAVING Year in (2014, 2015, 2016, 2017, 2018) 
                              ORDER BY Year, Month")
```
![crime_each_month](https://user-images.githubusercontent.com/72089707/124815351-2ad82080-df35-11eb-9b9b-58260a546583.png)

### The Number of Crimes at "Lower Manhattan", "Mid Manhattan", "Upper Manhattan"
Acquired the latitude and longitude bounding box from https://boundingbox.klokantech.com<br/>

Mid: -74.014776,40.730158,-73.95878,40.773077<br/>
Lower: -74.034071,40.691874,-73.967117,40.743045<br/>
Upper: -73.993637,40.758687,-73.916978,40.831108<br/>

```
crime_manhattan = spark.sql("SELECT SUBSTRING(Complnt_Date,7,4) year, 
                             SUM(CASE WHEN latitude BETWEEN 40.758687 AND 40.831108 AND longitude BETWEEN -73.993637 AND -73.916978 THEN 1 ELSE 0 END) Upper,                                    SUM(CASE WHEN latitude BETWEEN 40.730158 AND 40.773077 AND longitude BETWEEN -74.014776 AND -73.95878 THEN 1 ELSE 0 END) Mid, 
                             SUM(CASE WHEN latitude BETWEEN 40.691874 AND 40.743045 AND longitude BETWEEN -74.034071 AND -73.967117 THEN 1 ELSE 0 END) Lower 
                             FROM nyc_crime 
                             GROUP BY year 
                             ORDER BY year")
```
![bar_manhattan](https://user-images.githubusercontent.com/72089707/124994537-fdae6f80-e013-11eb-8d06-8b150c3fdd05.png)

```
upper = spark.sql("SELECT SUBSTRING(Complnt_Date,7,4) year, COUNT(*) upper_manhattan_cnt 
                   FROM nyc_crime 
                   WHERE latitude BETWEEN 40.758687 AND 40.831108 AND longitude BETWEEN -73.993637 AND -73.916978 
                   GROUP BY year 
                   ORDER BY year")
```
![upper](https://user-images.githubusercontent.com/72089707/124994570-099a3180-e014-11eb-8719-8a9a9f193adb.png)

```
mid = spark.sql("SELECT SUBSTRING(Complnt_Date,7,4) year, COUNT(*) mid_manhattan_cnt 
                 FROM nyc_crime 
                 WHERE latitude BETWEEN 40.730158 AND 40.773077 AND longitude BETWEEN -74.014776 AND -73.95878 
                 GROUP BY year 
                 ORDER BY year")
```
![mid](https://user-images.githubusercontent.com/72089707/124994589-128b0300-e014-11eb-866f-051c1fd8c004.png)

```
lower = spark.sql("SELECT SUBSTRING(Complnt_Date,7,4) year, COUNT(*) lower_manhattan_cnt 
                   FROM nyc_crime 
                   WHERE latitude BETWEEN 40.691874 AND 40.743045 AND longitude BETWEEN -74.034071 AND -73.967117 
                   GROUP BY year 
                   ORDER BY year")
```
![lower](https://user-images.githubusercontent.com/72089707/124994600-14ed5d00-e014-11eb-9c87-0d97678017e5.png)


### The Number of Crime in Each Hour in Certian Day like 2014/8/24, 2015/8/24, 2016/8/24, 2017/8/24, 2018/8/24
```
crime_certain_day = spark.sql("SELECT Complnt_Date, SUBSTRING(Complnt_Time, 0, 2) AS Hour, COUNT(*) AS Count 
                               FROM nyc_crime 
                               GROUP BY Complnt_Date, Hour
                               HAVING Complnt_Date IN ('08/24/2014', '08/24/2015', '08/24/2016', '08/24/2017', '08/24/2018') 
                               ORDER BY Hour, Complnt_Date")
```
![certain_day](https://user-images.githubusercontent.com/72089707/124848945-d69c6300-df6b-11eb-871a-29c1d27c56c8.png)

### Top Danger Boroughs and "ROBBERY" Count in Each Hour
```
crime_top3_boro = spark.sql("SELECT Borough, COUNT(*) AS Count 
                             FROM nyc_crime 
                             GROUP BY Borough 
                             ORDER BY Count DESC 
                             LIMIT 3")
```
```
crime_top3_boro.createOrReplaceTempView("crime_top3_boro")

crime_hour = spark.sql("SELECT Borough, SUBSTRING(Complnt_Time, 0, 2) AS Hour, COUNT(*) AS Count 
                        FROM nyc_crime 
                        WHERE Offence_Type == 'ROBBERY' 
                        AND Borough IN (SELECT Borough 
                                        FROM crime_top3_boro) 
                        GROUP BY Borough, Hour 
                        ORDER BY Borough, Hour")
```

![crime_hour](https://user-images.githubusercontent.com/72089707/124816054-0d578680-df36-11eb-8f83-262cede1fc24.png)

### K-Means Clustering for Spatial Analysis

<img width="660" alt="distribution" src="https://user-images.githubusercontent.com/72089707/125545475-ba2df131-186f-4602-8100-861d5ad16d66.png">

## Conclusion
The project aims to explore the criminal patterns in New York City, and provide valuable insights to visitors or even the police force. The project first preprocessed and manipulated the  NYPD complaint record using Pythong, then visulized the data using Spark SQL. Here are the major findings from this project:
1. The crime count is decreasing year over year, which means NYPD has done a great job
2. PETIT LARCENY, HARRASSMENT 2, and ASSAULT 3 & RELATED OFFENSES are the top three crime types in NYC
3. Brooklyn is the most dangerous borough
4. Jul and Aug usually have more crimes, and Dec, Jan and Feb are the safer months
5. Up-town Manhattan has the most crimes, but it shows a decending trend
6. Lower and Mid-town Manhattan have less crimes, but they've shown an increasing trend in recent years
7. The crime count is high from 3pm to 8 pm, and the number is significantly lower in the morning
