# **120-Years-of-Olympic-History-Data-Analysis-using-SQL**
## **Problem Statement:**
The purpose of this project is to provide a comprehensive overview of the trends and patterns in Olympic Games by considering information about participation, performance, sport, country, athletes, gender, age, height, and weight. The aim is to gain a more in-depth understanding of the data in compliance with SQL and Python.
## **Data Acquisition:**
The comprehensive dataset (Athlete_Events) of Olympic Games from 1896 Athens Olympics to 2016 Rio Olympics contains 271116 rows and 15 attributes.Each row corresponds to an individual athlete competing in an individual Olympic event.The attributes are as follows:<br />

•	**ID** - Unique number for each athlete <br />
•	**Name** - Athlete's name<br />
•	**Sex** - M or F<br />
•	**Age** - Integer<br />
•	**Height** - In centimeters<br />
•	**Weight** - In kilograms<br />
•	**Team** - Team name<br />
•	**NOC** - National Olympic Committee 3-letter code<br />
•	**Games** - Year and season<br />
•	**Year** - Integer<br />
•	**Season** - Summer or Winter<br />
•	**City** - Host city<br />
•	**Sport** - Sport<br />
•	**Event** - Event<br />
•	**Medal** - Gold, Silver, Bronze, or NA.<br />

The second dataset (Country_Definitions) has 230 rows and 3 attributes.The attributes are as follows:<br />

•	**NOC** - National Olympic Committee 3-letter code<br />
•	**Region** - Country<br />
•	**Notes** - Extra information about Country<br />

## **Data Cleaning:**
**•	Missing Values Handling:**<br />

4 out of 15 columns comprising Age, Height, Weight, and Medal have missing values. <br />

•Age - 9474<br />
•Height - 60171<br />
•Weight - 62875<br />
•Medal - 231332<br />

It is worth noting that Medal column has lots of NAN value because all sports have only three winners. In that matter, NaN values are replaced with "NWM" standing for "Not Won Medal". The rest of the numeric columns' missing values are replaced with mean values of their column.<br />

**•	Duplicated Values Handling:**<br />

In Athlete_Events dataset has 1385 duplicated records. By using Pandas library, duplicated records are dropped.

**•	Dropping Unnecessary Attributes:**<br />

Prior to data analysis of the dataset, column Games in Athlete_Events and column notes in Country_Definitions are removed due to redundancy.

## **Data Analysis:**
## **Results & Insights:**
