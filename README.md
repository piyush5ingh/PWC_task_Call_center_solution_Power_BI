PWC_task_Call_center_solution_Power_BI
PWC Call Center Analysis with Excel and Power BI

[Call Center](call_center_image.jpg)

## Overview

This project focuses on performing comprehensive analysis and visualization of call center data using Excel and Power BI. The goal is to gain valuable insights into call handling performance, customer satisfaction, and operational efficiency.

## Table of Contents

- [Project Description](#project-description)
- [Features](#features)
- [Prerequisites](#prerequisites)
- [Getting Started](#getting-started)
- [Usage](#usage)
- [Data Sources](#data-sources)
- [Visualization](#visualization)
- [Creating a Power BI Dashboard](#creating-a-power-bi-dashboard)
- [Contributing](#contributing)
- [Summary](#summary)

## Project Description

Managing and analyzing call center data is crucial for enhancing customer service and optimizing operational processes. This project offers a solution for analyzing call center performance by leveraging the power of Excel and Power BI.

## Features

- **Data Preparation**: Clean and transform raw call center data for analysis.
- **Excel Analysis**: Perform in-depth analysis and calculations using Excel.
- **Power BI Visualization**: Create interactive and insightful visualizations.
- **Customer Satisfaction Analysis**: Measure and visualize customer satisfaction scores.
- **Call Handling Efficiency**: Analyze and optimize call handling times and performance.
- **Historical Trends**: Identify historical trends and make data-driven decisions.

## Prerequisites

Before you begin, ensure you have met the following requirements:

- [Excel](https://www.microsoft.com/en-us/microsoft-365/excel) installed on your local machine.
- [Power BI Desktop](https://powerbi.microsoft.com/en-us/desktop/) for advanced data visualization.
- Access to the call center data source (e.g., Excel spreadsheet, database).

## Getting Started

To get started with this project, follow these steps:

1. Clone this repository to your local machine.
2. Prepare your call center data source (e.g., Excel file) with the required columns.
3. Open Excel and Power BI to perform data analysis and visualization.
4. Follow the instructions in the documentation to create visualizations and gain insights.

## Usage

This project can be used for:

- Call center performance analysis.
- Creating management reports and dashboards.
- Identifying areas for improvement in call handling and customer satisfaction.
- Making data-driven decisions for call center operations.

## Data Sources

The project uses the following data sources:

- Call center data in Excel format provided by PWC.
- Customer satisfaction surveys.

## Visualization

Sample visualizations from this project:
![image](https://github.com/piyush5ingh/PWC_task_Call_center_solution_Power_BI/assets/107373555/750d6e31-f0b6-4bc7-b9d0-621e9700342e)
![2](https://github.com/piyush5ingh/PWC_task_Call_center_solution_Power_BI/assets/107373555/a6bff7ba-6ee7-417f-885b-6dc1765248c3)
![3](https://github.com/piyush5ingh/PWC_task_Call_center_solution_Power_BI/assets/107373555/42a00f9e-0a84-4137-a668-1ff8e34f0153)

## Creating a Power BI Dashboard

To create a dashboard in Power BI for Claire that reflects all relevant Key Performance Indicators (KPIs) and metrics in the dataset, follow these steps:

1. Open Power BI Desktop.
2. Import the cleaned and transformed call center data into Power BI.
3. Create the following KPIs using DAX formulas:

   - Overall customer satisfaction
   - Overall calls answered/abandoned
   - Calls by time
   - Average speed of answer
   - Agent’s performance quadrant (average handle time vs. calls answered)

4. Design an interactive dashboard with visualizations that showcase these KPIs and metrics creatively.
5. Add filters, slicers, and drill-through options for Claire to explore the data.
6. Save and publish the Power BI report to a location accessible by Claire.

## DAX FARMULA 
1. **Overall Customer Satisfaction:**

   Formula:
   ```DAX
   Average Satisfaction Rating = AVERAGE([Satisfaction rating])
   ```

3. **Overall Calls Answered/Abandoned:**

   Formula for Calls Answered:
   ```DAX
   Total Calls Answered = COUNTROWS(FILTER('YourTableName', 'Answered (Y/N)' = "Y"))
   ```

   Formula for Calls Abandoned:
   ```DAX
   Total Calls Abandoned = COUNTROWS(FILTER('YourTableName', 'Answered (Y/N)' = "N"))
   ```

4. **Calls by Time:**

   Formula:
   ```DAX
   Calls by Time = COUNTROWS(FILTER('YourTableName', TIME(HOUR([Time]), MINUTE([Time]), SECOND([Time])) >= TIME(StartHour, StartMinute, StartSecond) && TIME(HOUR([Time]), MINUTE([Time]), SECOND([Time])) <= TIME(EndHour, EndMinute, EndSecond)))
   ```

   Replace `'YourTableName'`, `StartHour`, `StartMinute`, `StartSecond`, `EndHour`, `EndMinute`, and `EndSecond` with your actual column names and values.

5. **Average Speed of Answer (ASA):**

   Formula:
   ```DAX
   Average ASA (Seconds) = AVERAGE([Speed of answer in seconds])
   ```

6. **Agent's Performance Quadrant (Average Handle Time vs Calls Answered):**

   Formula for Average Handle Time:
   ```DAX
   Avg Handle Time = AVERAGE(DURATION([AvgTalkDuration]))
   ```

 7.  Formula for Calls Answered by Agent:
   ```DAX
   Total Calls Answered by Agent = COUNTROWS(FILTER('YourTableName', 'Answered (Y/N)' = "Y"))
   ```

8. // This measure calculates the Call Speed of Answer based on a moving average
 ```DAX
Call Speed of Answer =
VAR MovingAvg = AVERAGEX(
    FILTER(
        ALL('call centre trends'[Date]), // Filter the Date column to consider a 30-day window
        'call centre trends'[Date] >= EARLIER('call centre trends'[Date]) - 30 && 'call centre trends'[Date] <= EARLIER('call centre trends'[Date])
    ),
    'call centre trends'[Speed of answer in seconds] // Calculate the average of Speed of Answer in this window
)
RETURN
    SWITCH(
        TRUE(),
        MovingAvg >= 10 && MovingAvg <= 50, "High", // If the moving average is between 10 and 50, categorize as "High"
        MovingAvg >= 50 && MovingAvg < 100, "Medium", // If it's between 50 and 100, categorize as "Medium"
        MovingAvg >= 100 && MovingAvg < 130, "Low", // If it's between 100 and 130, categorize as "Low"
        "Abandoned" // Otherwise, categorize as "Abandoned"
    )
  ```
8. This code assigns a "Rating Name" based on the "Satisfaction rating" value in the "Call Centre Trends" dataset, assigning labels based on the rating values.
 ```DAX
 Rating Name =
IF('Call Centre Trends'[Satisfaction rating] = 1, "Poor",
   IF('Call Centre Trends'[Satisfaction rating] = 2, "Fair",
      IF('Call Centre Trends'[Satisfaction rating] = 3, "Average",
         IF('Call Centre Trends'[Satisfaction rating] = 4, "Good",
            IF('Call Centre Trends'[Satisfaction rating] = 5, "Excellent", "")
         )
      )
   )
)
  ```
9. Create a Time table
    
 ```DAX
Time table = 
VAR _SERIES = 
   GENERATESERIES( 1, 1440, 1 )
VAR _TIME = 
    ADDCOLUMNS( _SERIES, "TIMEANDDAY", TIME (0, [VALUE], 0))
RETURN
    ADDCOLUMNS(
        _TIME,
        "TIMEKEY", FORMAT([TIMEANDDAY], "hhmm"),
        "ACTUAL TIME", FORMAT([TIMEANDDAY], "HH:MM AM/PM"),
        "HOUR", HOUR([TIMEANDDAY]),
        "HOUR EXTENDED", FORMAT([TIMEANDDAY], "H AM/PM"),
        "MINUTE", MINUTE([TIMEANDDAY]),
        "AMPM", FORMAT([TIMEANDDAY], "AM/PM")
    )
 ```
Make sure to replace `'YourTableName'` with the actual name of your table in Power BI, and use the column names as indicated in your dataset.
## Contributing

Contributions are welcome! If you'd like to contribute to this project, please follow the [Contributing Guidelines](CONTRIBUTING.md).


## Summary

📊 Data Visualization Insights:
1. Most call satisfaction ratings are 3 and 4.
2. Average satisfaction rating decreased over three months, peaking in January and dipping in March.
3. Issue resolution rate was highest in January, dipped in February, and increased in March.
4. Majority of calls occur in the morning.
5. Joe has the highest average speed of answer.
6. Jim has the highest call resolution rate despite slower speed and higher call volume.
7. Becky has the slowest speed of answer but a higher call resolution rate, ranking 5th.
8. Martha has the highest speed of answer in the second position.

