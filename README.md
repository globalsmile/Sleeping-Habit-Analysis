# Client/Partner Sleeping Habit Analysis

<img align="right" alt="Snoring Partner" width="1000" height = "400" src="https://www.helpguide.org/wp-content/uploads/senior-man-covering-her-ears-while-man-snoring-picture-768.jpg">

---


# Table of Contents

- [Introduction](https://github.com/globalsmile/Airline-Analysis#introduction)
- [Problem Statement](https://github.com/globalsmile/Airline-Analysis#Problem-Statement)
- [Data Sourcing](https://github.com/globalsmile/Airline-Analysis#Data-Sourcing)
- [Data Transformation](https://github.com/globalsmile/Airline-Analysis#Data-Transformation)
- [Data Modeling](https://github.com/globalsmile/Airline-Analysis#Data-Modeling)
- [Data Visualization](https://github.com/globalsmile/Airline-Analysis#Data-Visualization)
- [Data Analysis](https://github.com/globalsmile/Airline-Analysis#Data-Analysis)
- [Recommendation](https://github.com/globalsmile/Airline-Analysis#Recommendation)
- [Shareable link](https://github.com/globalsmile/Airline-Analysis#Shareable-Link)

---

# Introduction
According to [WikiHow](https://www.wikihow.com/Survive-a-Plane-Crash):

The chances of dying on a commercial airline flight are actually as low as 9 million to 1. That said, a lot can go wrong at 33,000 feet (10,058.4 m) above the ground, and if you’re unlucky enough to be aboard when something does, the decisions you make could mean the difference between life and death. Almost 95% of airplane crashes have survivors, so even if the worst does happen, your odds aren't as bad as you might think

So let's see...

---

# Problem Statement

The main objective of this project is to perform an exploratory data analysis on the dataset and visualize the data to find some weird or interesting insights.

---

# Data Sourcing

This Dataset was created on Kaggle in September 2016 but the original version was hosted by Open Data by Socrata at:
https://opendata.socrata.com/Government/Airplane-Crashes-and-Fatalities-Since-1908/q2te-8cvq (no longer available). The dataset contains data of airplane accidents involving civil, commercial and military transport worldwide from `1908-09-17 to 2009-06-08`

Data:  Data can be accessed from this link  https://aka.ms/30DLDATGitHubRepo Locate project folder and download the csv file.
The dataset contains `13 columns and 5268 rows` of data.

---

# Data Transformation

For the purpose of this analysis, Microsoft Power BI was used to transform the data.

Data transformation begins in Power query after the data has been loaded into Microsoft Power BI.

- In Power query the table containing the dataset is named `Airplane_Crashes_and_Fatalities_since_1908`. It contains `13 columns and 5268 rows` data we will be working with.

The table below shows the column names and their description:
| Column Name | Description |
| ----------- | ----------- |
| Date | Represents the date of the accident |
| Time | Represents the time(local) of the accident in 24hrs and in the format hh:mm |
| Location | Represents the location of the accident |
| Operator | Represents the airline or the operator of the aircraft |
| Flight | Represents the flight ID assigned by the aircraft operator |
| Route | Represents the  complete or partial route flown prior to the accident |
| Type | Represents the aircraft type |
| Registration | Represents ICAO registration of the aircraft |
| Cn/Ln | Represents the construction or serial number/line or fuselage number |
| Aboard | Represents the total number of people aboard the plane |
| Fatalities | Represents the number of fatalities |
| Ground | Represnts the total number of people killed on the ground by the aircraft |
| Summary | Contains brief description of the accident and cause if known |

- To clean the data, power query was used to check the column's profile, quality, and distribution. it was found out that some columns were missing some values.

The table below shows the number of missing values in each of the columns:
| Column Name | No. of missing values |
| ----------- | ----------- |
| Date | 0 |
| Time | 684 |
| Location | 8 |
| Operator | 12 |
| Flight | 897 |
| Route | 531 |
| Type | 19 |
| Registration | 155 |
| Cn/Ln | 463 |
| Aboard | 0 |
| Fatalities | 0 |
| Ground | 0 |
| Summary | 243 |

- To account for the missing values, each column containing a missing value had to be manipulated.

The table below shows each of the column with a missing value and the kind of manipulation that was done:
| Column Name | Data manipulation |
| ----------- | ----------- |
| Time | Replaced the missing values with an arbitrary time values - (00:00) |
| Location | Replaced the missing values with 'unknown'|
| Operator | Replaced the missing values with 'unknown' |
| Flight | Replaced the missing values with 'unknown' |
| Type | Replaced the missing values with 'unknown' |
| Registration | Replaced the missing values with 'unknown' |
| Cn/Ln | Replaced the missing values with 'unknown' |
| Summary | Replaced the missing values with 'unknown' |

After the columns with missing values were manipulated, each column in the table `Airplane_Crashes_and_Fatalities_since_1908`  was then validated to have the correct data type to ensure data accuracy.

- Given that we have numerous dates and time in the dataset, a date table was needed so as to reference the date and the time more accurately.
A date table was created with the M-formula `List.Dates(#date(1908,09,17), 365*101, #duration(1,0,0,0)`

Here is a breakdown of what the formula does:

For `Airplane_Crashes_and_Fatalities_since_1908` data, we want the start date to reflect the earliest date that we have in the data: September 17, 1908. Additionally, you want to see date for the 101 years(time frame for our anlysis), including dates in the future.This approach ensures that, as new airplane crash and fatalities data flows in you won't have to re-create this table.Also the duration represents data point for everyday.

The date table was named `Calender` and the flight column was renamed to `Flight ID` for clarity.

---

# Data Modeling

After the data was cleaned and transformed, it was ready to be modeled.
- In the `Calender` table, `calculated columns` was used to extract the `Day`,` Month`, `Quarter`, `Year` columns from the table.

For `Day`, we used the DAX expression `FORMAT(Calender[Dates],'DDDD')`

For `Month`, we used the DAX expression `MONTH(Calender[Dates])`

For `Day`, we used the DAX expression `QUARTER(Calender[Dates])`

For `Day`, we used the DAX expression `YEAR(Calender[Dates])`

- An hierarchy was created in the `Calender` table to include the following columns: `Day`,` Month`, `Quarter`, `Year`
- The `Calender` table was then marked as the official date table in the dataset.
- To reference the date and time in the `Airplane_Crashes_and_Fatalities_since_1908` table more accurately, a `one-to-many (*:1) relationship` was created between the 
`Airplane_Crashes_and_Fatalities_since_1908` and the `Calender` table using the `Dates` column in each of the tables.

---

# Data Visualization

Data visualization for the dataset was done in two folds:
- `Annual Analysis`: Shows the number of accidents, fatalities, location, etc. for a previously selected year
- `Data Summary 1908 - 2009`: Shows all the data from the whole period, including the total number of accidents and total number of fatalities by year, operator and aircraft type.

- Figure 1 shows visualizations from `Annual Analysis`

| Figure 1 |
| ----------- |
| ![image](https://user-images.githubusercontent.com/106287208/180833146-0c593368-94fd-4550-9177-cec32cc32ab0.png) |

- Figure 2 shows visualizations from `Data Summary 1908 - 2009`

| Figure 2 |
| ----------- |
| ![image](https://user-images.githubusercontent.com/106287208/180864176-aee4c729-d6de-41e8-b2ca-68192671476c.png) |

---

# Data Analysis

- For [Figure 1](https://user-images.githubusercontent.com/106287208/180833146-0c593368-94fd-4550-9177-cec32cc32ab0.png), a measure was created using the DAX `Total Fatalities = SUM(Airplane_Crashes_and_Fatalities_since_1908[Aboard] + SUM(Airplane_Crashes_and_Fatalities_since_1908[Ground])` to aggregrate the total number of fatalities for each year.

see Figure 3 below:
| Figure 3 |
| ----------- |
| ![image](https://user-images.githubusercontent.com/106287208/180865767-0c9d6bf9-6032-4ec0-a932-39544596880c.png) |

- From [Figure 2](https://user-images.githubusercontent.com/106287208/180864176-aee4c729-d6de-41e8-b2ca-68192671476c.png), It looks like Aeroflot has the highest number of accidents for all the time specified by the dataset.

See Figure 4 below:
| Figure 4 |
| ----------- |
| ![image](https://user-images.githubusercontent.com/106287208/180882778-6b87c2f1-975f-4730-adb2-15609ddf3883.png) |

PJSC Aeroflot – Russian Airlines, commonly known as Aeroflot, is the flag carrier and largest airline of the Russian Federation. The carrier is an open joint stock company that operates domestic and international passenger and services, mainly from its hub at Sheremetyevo International Airport. (c) [Wikipedia](https://en.wikipedia.org/wiki/Aeroflot)

Also the data point for fatalities on ground for the year 2001 appears to look like an outlier, however the 9/11 attacks against targets in the United States contibuted to a massive rise in total fatalities (on ground) for the year.

See Figure 5 below:
| Figure 5 |
| ----------- |
| ![image](https://user-images.githubusercontent.com/106287208/180882364-a55df9fc-f344-43d1-9779-ae38bf74cb8c.png) |

From our 'Annual Analysis' for the year 2001 it is estimated that they were a total of:

`60 accidents`, `7752 fatalities`, `2111 people killed abord`, `5641 people killed on the ground`.
See Figure 6 below:
| Figure 6 |
| ----------- |
| ![image](https://user-images.githubusercontent.com/106287208/180875463-3b6f81c7-242f-433d-9daf-01d9d66f1559.png) |

It was also found that, about `2880 people were killed on ground` from the 9/11 attacks against target in the USA.
Find out more at [9/11 attacks](https://www.britannica.com/event/September-11-attacks)

Generally for the dataset, they were a total of `5268 Accidents` resulting in about `153K Fatalities` spanning a period of `102` years.


---

# Recommendation

---

# Shareable Link

You can interact with the report here: 

https://app.powerbi.com/view?r=eyJrIjoiZWEzMTE3ZjUtZmZhOS00YWQ0LThiM2EtMjBiZmQ2ZjFiNWZlIiwidCI6IjQ5ODY4YWYzLWNjNWYtNDIxNC04YjdmLTQwZjM3NDY0OWEwOSJ9&pageName=ReportSection19294c9f61400f986291

