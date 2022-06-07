# Bellabeat_Case_Study

FROM `bellabeat2022.Bella_Beat2022.Daily_Calories ` 

#Used to find how many unique Ids were found in the Daily Calories data sheet 
# 33 Uniques Ids

SELECT DISTINCT Id
FROM `bellabeat2022.Bella_Beat2022.Daily_Activity `

#Used to find how many unique Ids were in the Daily Activity Data sheet 
# 33 unique Ids 

SELECT MIN (Activitydate) AS StartDate, MAX(Activitydate) AS EndDate
  FROM `bellabeat2022.Bella_Beat2022.Daily_Activity `
 # The start date was 04-12-2016 
 # The end date was 05-12-2016

SELECT DISTINCT Id
FROM `bellabeat2022.Bella_Beat2022.Daily_Step`

# 33 Unique Ids found 

SELECT MIN (Activityday) AS StartDate, MAX(Activityday) AS EndDate
  FROM `bellabeat2022.Bella_Beat2022.Daily_Step`

 # The start date was 04-12-2016 
 # The end date was 05-12-2016 

SELECT DISTINCT Id
FROM `bellabeat2022.Bella_Beat2022.Weight_Log`

# USed to find the unique Ids in the data
# There are 8 unique Ids 

SELECT Id, AVG(Calories) AS Avg_Calories
FROM `bellabeat2022.Bella_Beat2022.Daily_Calories` 
GROUP BY Id
 
# Used to find the average calories burned by each individual over the month 

;
 
SELECT  Id, ROUND(AVG(Calories), 0) AS AVG_Calories, Day_Type
FROM 
(SELECT 
  Id, 
  Calories,
  CASE 
    WHEN day_of_week = 1 or day_of_week = 7 THEN 'Weekend'
    ELSE 'Weekday'
  END AS Day_Type,
  CASE 
    WHEN day_of_week = 1 THEN 'Sunday'
    WHEN day_of_week = 2 THEN 'Monday'
    WHEN day_of_week = 3 THEN 'Tuesday'
    WHEN day_of_week = 4 THEN 'Wednesday'
    WHEN day_of_week = 5 THEN 'Thursday'
    WHEN day_of_week = 6 THEN 'Friday'
    WHEN day_of_week = 7 THEN 'Saturday'
    END AS Day_of_Week
 FROM 
(
  SELECT 
  EXTRACT(DAYOFWEEK from ActivityDay) AS day_of_week, Id, Calories
  FROM `bellabeat2022.Bella_Beat2022.Daily_Calories`
))
GROUP BY Day_Type, Id   
 
#Used to find the average calories burned by individuals on the weekday vs the weekend 

;

SELECT Id, ROUND(AVG(VeryActiveDistance), 2) AS Avg_VeryActiveDistance, ROUND(AVG(ModeratelyActiveDistance), 3) AS Avg_ModeratelyActiveDistance, ROUND(AVG(LightActiveDistance),3) AS Avg_LightActiveDistance, ROUND(AVG(SedentaryActiveDistance), 3) AS Avg_SedentaryActiveDistance
FROM `bellabeat2022.Bella_Beat2022.Daily_Activity `
GROUP BY Id
 
#Used to find avg distance of each Id over the month 
 
SELECT Id, ROUND(AVG(Total_Minutes_Asleep), 0) AS Avg_Minutes_Asleep, ROUND(AVG(Minutes_Awake_In_Bed), 0) AS Avg_Minutes_Awake_In_Bed
FROM `bellabeat2022.Bella_Beat2022.Sleep_Log` 
GROUP BY Id 
ORDER BY Avg_Minutes_Awake_In_Bed
 
 
#Used to find the average minutes of sleep individuals were receiving and average minutes awake in bed.

;

SELECT
  Avg_Daily_Steps.Id AS User_Id,
  Avg_Daily_Steps.AVG_StepTotal AS Avg_Step_Total,
  Avg_Daily_Steps.Day_Type AS Day_Type,
  Avg_Sleep_Log.Id AS User_Id,
  Avg_Sleep_Log.Avg_Minutes_Asleep AS AVG_Minutes_Asleep,
  Avg_Sleep_Log.Avg_Minutes_Awake_In_Bed AS AVG_Minutes_Awake_In_Bed,
  FROM 
  `bellabeat2022.Bella_Beat2022.Avg_Daily_Steps` AS Avg_Daily_Steps
  INNER JOIN
  `bellabeat2022.Bella_Beat2022.Avg_Sleep_Log` AS Avg_Sleep_Log ON
  Avg_Daily_Steps.Id = Avg_Sleep_Log.Id
  ORDER BY Avg_Daily_Steps.Id 
 
SELECT Id, ROUND(AVG(Steptotal),0) AS Avg_Daily_Step_Total
FROM `bellabeat2022.Bella_Beat2022.Daily_Steps`
GROUP BY Id

#Joining Daily Steps and Sleep Log Tables 

;

SELECT
  Avg_Calories.Id AS User_Id,
  Avg_Calories.Avg_Calories AS AVG_Daily_Calories,
  Average_Daily_Steps.Id AS User_Id,
  Average_Daily_Steps.Avg_Daily_Step_Total AS Avg_Step_Total,
  FROM 
  `bellabeat2022.Bella_Beat2022.Average_Daily_Steps` AS Average_Daily_Steps
  INNER JOIN
  `bellabeat2022.Bella_Beat2022.Avg_Calories` AS Avg_Calories ON
  Average_Daily_Steps.Id = Avg_Calories.Id
  ORDER BY Average_Daily_Steps.Id 
 
# Joining Average Daily Steps and Average Calories tables 

SELECT   ROUND(AVG(StepTotal), 0)  AS Avg_StepTotal, Day_of_Week
FROM 
(SELECT 
  
  StepTotal,
  CASE 
    WHEN day_of_week = 1 THEN 'Sunday'
    WHEN day_of_week = 2 THEN 'Monday'
    WHEN day_of_week = 3 THEN 'Tuesday'
    WHEN day_of_week = 4 THEN 'Wednesday'
    WHEN day_of_week = 5 THEN 'Thursday'
    WHEN day_of_week = 6 THEN 'Friday'
    WHEN day_of_week = 7 THEN 'Saturday'
    END AS Day_of_Week
 FROM 
(
  SELECT 
  EXTRACT(DAYOFWEEK from ActivityDay) AS Day_of_Week, StepTotal
  FROM `bellabeat2022.Bella_Beat2022.Daily_Steps`
))
GROUP BY Day_Of_Week

#Used to find the Average Steps taken on each day of the week 


SELECT   ROUND(AVG(Total_Minutes_Asleep), 0)  AS Avg_StepTotal, Day_of_Week
FROM 
(SELECT 
  
  Total_Minutes_Asleep,
  CASE 
    WHEN day_of_week = 1 THEN 'Sunday'
    WHEN day_of_week = 2 THEN 'Monday'
    WHEN day_of_week = 3 THEN 'Tuesday'
    WHEN day_of_week = 4 THEN 'Wednesday'
    WHEN day_of_week = 5 THEN 'Thursday'
    WHEN day_of_week = 6 THEN 'Friday'
    WHEN day_of_week = 7 THEN 'Saturday'
    END AS Day_of_Week
 FROM 
(
  SELECT 
  EXTRACT(DAYOFWEEK from Sleep_Day) AS Day_of_Week, Total_Minutes_Asleep 
  FROM `bellabeat2022.Bella_Beat2022.Sleep_Log`
))
GROUP BY Day_Of_Week

#Used to Find what day of the week individuals slept the most 
