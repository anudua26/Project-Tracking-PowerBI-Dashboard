# Project-Tracking-PowerBI-Dashboard
Project Tracking Dashboard
Purpose: To provide a real-time overview of project status.To track key performance indicators (KPIs) like progress, budget, and deadlines. To identify bottlenecks or issues early in the project.

Key Components:
Project Overview

Project name, start & end dates, current phase/status.
Visual representation of milestones, tasks, and deadlines.

Task Status
Categorization of tasks (To Do, In Progress, Completed).
Budget Tracking
Planned vs. actual cost.

Datasource: Excel workbook. Sheets included: Cost, Schedule, Location. 
I treated Cost table as fact and the other two as dimensions. I used DAX Calendar function to create a dimension for Calendar. Also, created a separate table inside this data model to store all the DAX measures in one place. Data model can be seen under section Data Modelling below.

Data cleaning done by me inside Power Query Editor:
 • Handling Missing Values – replace nulls with default.
 • Split column by delimiter – Location table, column – Location into street 
name, city, postcode.
 • Renaming columns – clear, easy to read (Location, Cost  tables).
 • Removing extra spaces – using TRIM function (Location column in Location 
table).
 • Changing data types – Date datatype to the Actual Start and Actual Finish 
columns in Cost table

Data modelling 
(Star schema)
 • Cost table has the numeric data (actual and budget) so it’s the fact/data 
table that connects to other entities/dimensions
 • Connected the Schedule and Cost tables (lookup tables/dimensions) via 1 
to many relationship on the common key Activity ID 
• Used RELATED function to fetch the common keys related to Calendar and 
Location tables to get 1 to many relationship (1 on dim and many on Cost)
 • Example: RELATED('Schedule'[LocationID]) to get LocationID in Cost table
 • Hide the foreign keys inside the Cost table from report view
 • Setting up the data model layout as per best practice i.e. Collie layout 
methodology

<img width="1132" height="737" alt="image" src="https://github.com/user-attachments/assets/1a84ea3f-7f9d-419e-b956-91b048f32437" />


Calculated Columns
 (Using DAX)
 • Table Cost: Added columns: Variance, Cost status (SWITCH)
 • Table Cost: added foreign keys – Location ID, DateKey (RELATED)
 • Table Schedule: Duration Days (DATEDIFF), Is Delayed (IF), and two other 
columns using FORMAT function

Calculated Measures
 (Using DAX)
 • Created a new table (#Measure) to hold all DAX measures 
• Total Actual Cost = SUM(Cost[Actual]), Total Budget = SUM(Cost[Budget])
 • Variance, Variance%
 • Average Duration = AVERAGE('Schedule'[Duration Days])
 • Delayed % for activities
 • Actual Current Year, Budget Current Year, Actual Last Year using Time 
intelligence functions  (we can use Fiscal year “6/30” if required)
 • Time intelligence functions can also be used to see actual cost for previous 
year (PREVIOUSYEAR), and calculate YOY growth 
• Similarly, Quarter-on-Quarter Growth, Month-on-Month trends can also be 
achieved using DAX functions 

Dashboard design
 • Meaningful title to the dashboard
 • Putting the KPI and card visuals on the top
 • Putting slicers on the left side to provide more space to visuals
 • Clustered chart – to show the financial trends
 • Donut chart to show the total activities breakdown by budget category
 • Activities details with informative columns in table visual
 • Added conditional formatting, and bookmark (reset filters on page)
 • Added a text box: Note at the bottom of dashboard – to inform users on any assumptions used for better understanding

Key Insights from this dashboard
 • How are the key performance indicators progressing? 
• How many activities are going overbudget this year?
 • How many projects are delayed?– duration days, delayed%
 • How many high risks projects are there?  --- Overbudget, Is Delayed = Yes 
• Did we exceed budget in any month this year? If yes, which month/s?
 • How many or which projects/activities were completed on weekend? --overtime expenses
 • Which location is going overbudget and in which project?
 • Using Date, year, month slicers we can slice and dice the data in this report
 • Red background color in Variance, variance% - action items that might need attention

 <img width="1312" height="726" alt="image" src="https://github.com/user-attachments/assets/9ea10f8b-5df6-4e1f-ac89-4444857fcba2" />
















<img width="1312" height="726" alt="image" src="https://github.com/user-attachments/assets/58fe616b-1150-4ecd-af4b-944bf58862b8" />
