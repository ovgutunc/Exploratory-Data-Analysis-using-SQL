# **120-Years-of-Olympic-History-Exploratory Data-Analysis-using-SQL**
## **Problem Statement:**
The purpose of this project is to provide a comprehensive overview of the trends and patterns in Olympic Games by considering information about participation, performance, sport, country, athletes, gender, age, height, and weight. The aim is to gain a more in-depth understanding of the data in compliance with SQL and Python.<br>

To get further information about data analysis and visualization of the datasets, please check out **[Jupyter Notebook](https://github.com/ovgutunc/120-Years-of-Olympic-History-Data-Analysis-using-SQL/blob/main/olympics_history_data_analysis.ipynb).**  
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

The link to the datasets is **[here](https://www.kaggle.com/datasets/heesoo37/120-years-of-olympic-history-athletes-and-results).**

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

## **Data Analysis Using SQL:**

To get better insights about the dataset, SQL queries and Python are used to complement each other. The SQL queries are provied as follows:<br>

**1) What are the top 10 countries which have won medals the most ?** <br>
```
SELECT cd.Country,COUNT(ac.Medal) AS "Total Medal Number" FROM athlete_events_clean ac
JOIN country_definitions cd ON ac.NOC=cd.NOC
WHERE ac.Medal!="MNW" GROUP BY cd.Country ORDER BY COUNT(ac.Medal) DESC LIMIT 10 ;
```

**2) What is the break-up of medal tallies by medal types for the top 5 countries ?**<br>
```
SELECT cd.Country,ac.Medal,COUNT(ac.Medal) AS "Total Medal Number" FROM athlete_events_clean ac
JOIN country_definitions cd ON ac.NOC=cd.NOC
WHERE ac.Medal IN ("Gold","Bronze","Silver") AND cd.Country IN("USA","Russia","Germany","UK","France")
GROUP BY cd.Country,ac.Medal ORDER BY cd.Country DESC, COUNT(ac.Medal) DESC ;
```
**3) In which sports do the top 5 countries have the best performance ?**<br>
```
SELECT Country,Sport,MAX(medal_count) AS "Number of Medal"
FROM
    (SELECT cd.Country,ac.Sport,COUNT(ac.Medal) AS medal_count FROM athlete_events_clean ac
     JOIN country_definitions cd ON ac.NOC=cd.NOC
     WHERE ac.Medal!="MNW" AND cd.Country IN("USA","Russia","Germany","UK","France")
     GROUP BY ac.sport,cd.Country ORDER BY medal_count DESC) AS sub
GROUP BY Country;
```
**4) What are the top 5 countries that have won the most gold medals?**<br>
```
SELECT cd.Country,COUNT(ac.Medal) AS "Gold Medal Number" FROM athlete_events_clean ac
JOIN country_definitions cd ON ac.NOC=cd.NOC
WHERE ac.Medal="Gold" GROUP BY cd.Country ORDER BY COUNT(ac.Medal) DESC LIMIT 5 ;
```
**5) How is the number of medalists changing in Summer Olympics by gender over the years ?**<br>
```
SELECT ac.Year,ac.Sex,COUNT(ac.Medal) AS "Medal Number" FROM athlete_events_clean ac
JOIN country_definitions cd ON ac.NOC=cd.NOC
WHERE ac.Medal!="MNW" AND ac.Season="Summer" GROUP BY ac.Year,ac.Sex ORDER BY ac.Year DESC,ac.Sex DESC;
```
**6) How is the participation of athletes changing by gender in Summer Olympics over the years ?**<br>
```
SELECT ac.year,ac.Sex,COUNT(DISTINCT(ac.name)) AS "Number of Athletes" FROM athlete_events_clean ac
JOIN country_definitions cd ON ac.NOC=cd.NOC
WHERE ac.Season="Summer" GROUP BY ac.Year,ac.Sex ORDER BY ac.Year DESC;
```
**7) What are the height and weight of medalists by sport ?**<br>
```
SELECT Name,Sport,Height,Weight FROM athlete_events_clean 
WHERE Medal!="MNW" GROUP BY Name ORDER BY Sport;
```
**8) What are the average Weight/Height ratios of medalists by sport ?**<br>
```
SELECT Sport,AVG(Weight/Height) AS "Average of Weight/Height Ratios" FROM athlete_events_clean 
WHERE Medal!="MNW" GROUP BY Sport ORDER BY AVG(Weight/Height) DESC;
```
**9) What are the distribution of Height and Weight of medalists for Rhythmic Gymnastics and Weightlifting ?**<br>
```
SELECT Name,Sport,Height,Weight FROM athlete_events_clean 
WHERE Medal!="MNW" AND Sport IN("Rhythmic Gymnastics","Weightlifting")
GROUP BY Name ORDER BY Sport;
```
**10) What are the most popular Sports in Olympics ?**<br>
```
Select Sport, COUNT(DISTINCT(name)) AS "Number of Athletes" FROM athlete_events_clean
GROUP BY Sport ORDER BY COUNT(DISTINCT(name)) DESC LIMIT 5;
```
**11) Who has the most Olympic medals among female?**<br>
```
SELECT name AS Name,Sport,COUNT(Medal) AS "Number of Medals" FROM athlete_events_clean
WHERE Sex="F" AND Medal!="MNW" GROUP BY name,sport ORDER BY COUNT(Medal) DESC LIMIT 3;
```
**12) Who has the most Olympic medals among male?**<br>
```
SELECT name AS Name,Sport,COUNT(Medal) AS "Number of Medals" FROM athlete_events_clean
WHERE Sex="M" AND Medal!="MNW" GROUP BY name,sport ORDER BY COUNT(Medal) DESC LIMIT 3;
```
**13) Who is the first youngest athlete with a gold medal and in which sport?**<br>
```
SELECT age,name,sport,year FROM athlete_events_clean
WHERE medal="Gold" AND age=
                            (SELECT MIN(age) FROM athlete_events_clean WHERE medal="Gold")
ORDER BY year;
```
**14) Who is the oldest athlete with a gold medal and in which sport?**<br>
```
SELECT age,name,sport,year FROM athlete_events_clean
WHERE medal="Gold" AND age=
                        (SELECT max(age) FROM athlete_events_clean WHERE medal="Gold");
```
**15) Which sports are seniors participated in Olympics ? (60+)**<br>
```
SELECT sport,COUNT(DISTINCT(name)) AS "Number of Athletes" FROM athlete_events_clean
WHERE age>60 GROUP BY sport ORDER BY COUNT(DISTINCT(name)) DESC;
```

## **Results & Insights:**

•	The overall trend in the number of athletes has been gradually increasing for both female and male athletes over the years. In fact, the trend rate of female participation has been increasing significantly since 1980.However, 1932, 1956, and 1980 experienced a drastic fall in male participation. It can be explained by two main reasons:<br>

>-In 1932 Olympics, due to Great Depression, many countries were unable to afford to send teams to compete, resulting in a lower number of participations.<br>
>-In 1956 and 1980 Olympics, because of the boycotts of some countries over political issues, the participation was highly restricted.<br>

•	The success of female athletes has been constantly growing in terms of medal count in Summer Olympics. Over the last 5 Olympics, female and male athletes have won almost the same amount of medal. Now, women represent themselves and their countries equally.<br>

•	The United States of America, Russia, and Germany are medal tally leaders from 1896 to 2016 Olympics. They have the best performance in Athletics, Gymnastics, and Rowing respectively. It can be a result of investing highly in sports development and training, and having an opportunity to participate consistently.<br>

•	Athletics, Swimming, Rowing, Football, and Cycling can be considered the most popular sports in Olympics from the perspective of the number of athletes as a metric.<br>

•	Most athletes show a linear relationship between weight and height pertaining to the sport. However, Sports like Weightlifting have a higher weight/height ratio while other sports like Rhythmic Gymnastics have a lower weight/height ratio.<br>

•	The athletes holding the world record from 1896 to 2016 Olympics are as follows:<br>

>-Larisa Semyonovna Latynina with the most Olympic gold medals as a female gymnast<br>
>-Michale Phelps with the most Olympic gold medals as a male swimmer<br>
>-Aileen Muriel Riggin with the youngest Olympic gold medalist at 13 as a female diver<br>
>-Charles Jacobus with the oldest Olympic gold medalist at 64 as a male roque player<br>

_Thank you for your precious time.I hope you enjoy my data world as much as ı do._<br>
_Keep grinding !_
