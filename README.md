# BigQuery-Data-Exploration-Project
![Image](https://github.com/user-attachments/assets/2557c2ca-cff2-41a2-a7db-4ad755da2e39)

BigQuery Data Exploration Part 1

Step 1 - While exploring the dataset, look at the Schema tab and read through the descriptions provided for each column, to get a sense of what data is in this table. This will help you answer the questions below.

Step 2 - Answer the following questions

There are several acronyms used in this dataset. For each acronym below, write out what it stands for:
CoC =  Continuum of Care Area 
Sheltered_ES = Homeless in Emergency Shelters
Sheltered_TH = Homeless in Transitional Housing
Sheltered_SH = Homeless in Safe Haven Housing
What are the only 3 columns that are NOT an Integer type?
CoC_Number
CoC_Name
CoC_Category

Is there any way to determine which state each row of data is located in?
no, there is a way to determine which state each row of data is located in

How many total rows of data are there?
There are 50 rows of data

What might be some reasons that someone would use this dataset (no wrong answers here, just trying to think about how this data could be used)?
This data assesses the homelessness in America, estimates of chronically homeless persons, homeless veterans, and homeless children and youth.
“These data can be trended over time and correlated with other metrics of housing availability and affordability, in order to better understand the particular type of housing resources that may be needed from a social determinants of health perspective.”



Step 3 - While you have the dataset selected, click on the Query button, and choose “In New Tab”. In the new query window, you can delete all of the SQL code there, and copy and paste the following code, then run it to create a new table:
```sql
CREATE TABLE Exploration_Project.homelessness AS
SELECT CoC_Number, LEFT(CoC_Number, 2) AS State, CoC_Name, Overall_Homeless, Sheltered_ES_Homeless, Sheltered_TH_Homeless, Sheltered_SH_Homeless, Sheltered_Total_Homeless, Unsheltered_Homeless, Homeless_Individuals, Homeless_People_in_Families, Chronically_Homeless, Homeless_Veterans, Homeless_Unaccompanied_Youth_Under_18, Count_Year
FROM `bigquery-public-data.sdoh_hud_pit_homelessness.hud_pit_by_coc`
```


Do a quick Google search to figure out what the SQL function LEFT( ) does. In the query we just ran, what does the line of code, LEFT(CoC_Number, 2) AS State, do?
Creating a table in exploration_project names homelessness form the public dataset on homelessness.
We then create a “State” column based on the “CoC_Number” indicated.
Put “State” to the left of “CoC_Number” and name it “State”
Also export these different rows “ CoC_Name, Overall_Homeless, Sheltered_ES_Homeless, Sheltered_TH_Homeless, Sheltered_SH_Homeless, Sheltered_Total_Homeless, Unsheltered_Homeless, Homeless_Individuals, Homeless_People_in_Families, Chronically_Homeless, Homeless_Veterans, Homeless_Unaccompanied_Youth_Under_18, Count_Year”


Step 4 - Open the new table you just created and use the Preview tab to look at the data and make sure all of the columns are appearing properly. 

BigQuery Data Exploration Part 2

Step 1 - Open the dataset you created last time, which should be a table named Homelessness. Once you have opened it, click on the Query button and open a new query in a new tab.

Step 2 - Using this dataset, answer the following questions. These first few questions can be answered by using ORDER BY and WHERE clauses. For reference, a list of all of the state abbreviations is listed HERE. 

1. We want to develop some new programs to help unaccompanied homeless children under 18 years old, and need some locations to start some programs. What are the top 3 Continuum of Care areas (CoC_Name in the table) with the highest number of unaccompanied homeless youth under 18 in the year 2018.

Find the top 3 areas (CoC_Name) with the highest number of homeless youth under 18 who are on their own (unaccompanied) in 2018.

Answer:
San Jose/ Santa Clara City & County CoC
Oregon Balance of State CoC
Las Vegas/ Clark County CoC

SQL Code Used:
```sql
SELECT CoC_Name, Homeless_Unaccompanied_Youth_Under_18 #Find this
FROM `Exploration_Project.homelessness` #Get the data
WHERE Count_Year = 2018 #year 2018
ORDER BY Homeless_Unaccompanied_Youth_Under_18 DESC #unacc. in desc. order
LIMIT 3; #top 3
```

2. We suspect that in Delaware (state abbreviation is “DE”), the number of unsheltered homeless people has been increasing over the past 7 years. Is this statement true? How do you know?
Look at Delaware's data over 7 years to see if the number of unsheltered homeless people has been increasing.

Answer:
Yes, the number of unsheltered homeless people has been increasing

SQL Code Used:
```sql
SELECT Count_Year, Unsheltered_Homeless, State,
FROM `Exploration_Project.homelessness`
WHERE State = "DE"
ORDER BY Count_Year ASC;
```


3. The Safe Haven program was created in 1992 to provide shelter for people who are homeless and have a serious mental illness. However, funding for Safe Havens was cut in 2009, so no new Safe Havens could be created. Looking at data from only 2018, answer the following questions:
About Safe Haven programs (shelters for homeless people with serious mental illness):
In 2018, how many different locations had at least 1 person as a Sheltered_SH?
How many locations had at least one person in Safe Haven shelters in 2018?

Answer:
In the year 2018 there are 50 different locations that had at least 1 person as a Sheltered_SH

SQL Code Used:
```sql
SELECT CoC_Name, Sheltered_SH_Homeless
FROM `Exploration_Project.homelessness`
WHERE Sheltered_SH_Homeless >= 1 AND Count_Year = 2018;
```

In 2018, what was the CoC_Name of the top 3 locations in terms of number of Sheltered_SH?
Which 3 locations had the most people in Safe Haven shelters in 2018?

Answer:
Philadelphia CoC
Reno, Sparks/Washoe County CoC
Indianapolis Coc 

SQL Code Used:
```sql
SELECT CoC_Name, Sheltered_SH_Homeless
FROM `Exploration_Project.homelessness`
WHERE Sheltered_SH_Homeless >= 1 AND Count_Year = 2018
ORDER BY Sheltered_SH_Homeless DESC
LIMIT 3;
```

4. What are the top 7 states in terms of Overall Homeless population in the year 2018? (Hint: This question will require the use of a GROUP BY clause in your query)
Find the 7 states with the highest total homeless population in 2018.

Answer:
CA, NY, FL, TX, WA, MA, OR

SQL Code Used:
```sql
SELECT State, SUM(Overall_Homeless)  AS Total_Homeless
FROM `Exploration_Project.homelessness`
WHERE Count_Year = 2018
GROUP BY State
ORDER BY Total_Homeless DESC
LIMIT 7;
```


5. Below you will see a table of all of the states and their populations in 2018. If the Overall Homeless population were only correlated with the total state population, then the top 7 states for total population and the top 7 states for Overall Homeless population would be the same. 
Compare state population rankings with homeless population rankings

Is this the case? If not, which states are not lining up the same?
Are the top 7 states the same in both lists?
No there are some states that aren’t lining up the same 

![Image](https://github.com/user-attachments/assets/b5f0aa99-3e04-4821-91bb-92f27a5190cd)

We would say that homelessness is overrepresented in a state if that state listed higher in the Homeless ranking than it did in total population ranking, and it would be underrepresented if the homeless ranking was lower than the population ranking. With that in mind, which states in the top 7 homelessness list would be overrepresented for homelessness?
Which states have more homelessness than you'd expect based on their population?

Population
California (CA) = 39776830


Homelessness
California (CA) = 129972



6. In order to create better policies to support homeless individuals, we want to study locations that are doing a good job providing shelter. To do so, we need to identify places that have a relatively large number of homeless, but a relatively small number of unsheltered homeless.
Looking for successful shelter programs

Generate a list of all locations, in 2018, that have more than 1000 Overall Homeless, but less than 100 unsheltered homeless. 
Find places that in 2018 had:Over 1,000 total homeless people But fewer than 100 unsheltered homeless people

Answer:
Nassau, Suffolk Counties CoC
Springfield/ Hampden County CoC
Maine Statewide CoC
Yonkers, Mount Vernon/Westchester County CoC
Cleveland/Cuyahoga County CoC
Omaha, Council Bluffs CoC
Cincinnati/Hamilton County CoC
Rhode Island Statewide CoC
Anchorage CoC
Quincy, Brockton, Weymouth, Plymouth City and County CoC	
Delaware Statewide CoC	
Lynn CoC

SQL Code Used:
```sql
SELECT  CoC_Name,
        State,
        Overall_Homeless,
        Unsheltered_Homeless
FROM `Exploration_Project.homelessness`
WHERE Count_Year = 2018
      AND Overall_Homeless >= 1000
      AND Unsheltered_Homeless <= 100
ORDER BY Overall_Homeless DESC;
```


From that same list, in which locations do Unsheltered Homeless account for less than 2% of the Overall Homeless population?
Among these places, find ones where unsheltered homeless make up less than 2% of the total homeless population

Answer:
Springfield/ Hampden County CoC
Nassau, Suffolk Countries CoC

SQL Code Used:
```sql
SELECT  CoC_Name,
        Overall_Homeless,
        Unsheltered_Homeless,
        -- Calculate the percentage of homeless who are unsheltered
        -- Multiply by 100.0 (not 100) to force floating point division
        -- ROUND to 2 decimal places for cleaner output
        ROUND((Unsheltered_Homeless * 100.0 / Overall_Homeless), 2) AS Unsheltered_Percentage
FROM `Exploration_Project.homelessness`
WHERE Count_Year = 2018
      AND Overall_Homeless > 1000
      AND Unsheltered_Homeless < 100
      -- Calculate percentage and filter for less than 2%
      -- This is the same calculation as above but used as a filter
      AND (Unsheltered_Homeless * 100/ Overall_Homeless) < 2
ORDER BY Unsheltered_Percentage ASC;
```










