# Online Sales - Dashboard

### Dashboard Link: https://app.powerbi.com/groups/me/reports/e0280a5f-0272-4c9c-91d1-84ee20707487/ReportSection

# Problem Statement

This dashboard focuses on analyzing the monthly trends, top-selling products, best-performing product categories, predominant market regions, and preferred payment methods. This comprehensive analysis provides valuable insights into sales performance, customer preferences, and market dynamics, enabling data-driven decision-making and strategic planning to drive business growth and optimize operational strategies.

### Steps followed

- Step 1: Load data into Power BI Desktop, dataset is a csv file.

- Step 2: It was observed that the Transaction ID column contained empty values, and the Date column was set as Text Data Type. Power Query Editor was used to remove the empty values and change the data type of the Date column.

- Step 3: A calendar table was created using the following DAX expression.

        Calendar Table = 
        Var StartDate = FIRSTDATE('Online Sales Data'[Date])
        Var FinishDate = LASTDATE('Online Sales Data'[Date])
        Var VarDate = CALENDAR(StartDate,FinishDate)
        RETURN

        SELECTCOLUMN(VarDate,
        "Date", [Date],
        "Year", YEAR([Date]),
        "MonthNum",MONTH([Date]),
        "Day",DAY([Date]),
        "Quarter", QUARTER([Date]),
        "Month", FORMAT([Date],"MMMM"),
        "WeekDay",FORMAT(WEEKDAY([Date]),"DDDD"),
        "WeekNum",WEEKDAY([Date],2)
        )

Snap of new table,

> ![Calendar Table](https://github.com/user-attachments/assets/4ec352ba-9980-49da-8f02-3ce094de252d)

- Step 4: A new relationship was created (one-to-many with single direction) between Calendar Table and Online sales Data.

Snap of relationship between tables,

> ![Relationship](https://github.com/user-attachments/assets/f669e849-b7e5-4de3-b113-2b797b27496e)

- Step 5: A Line and clustered column chart was added to the report design area using Total Revenue column and Month Column to represent sales trends over time to identify seasonal patterns or growth opportunities.

- Step 6: New measure was created to find % of difference between current month and previous one.

Following DAX expression was written to find % of diffrence:

        TrendOverMonth = 
        Var CurrentMonth = SUM('Online Sales Data'[Total Revenue])
        Var LastMonth = CALCULATE(SUM('Online Sales Data'[Total Revenue]),PREVIOUSMONTH('Online Sales Data'[Date]),VALUES('Online Sales Data'[Product Category]))
        Var VarTrend = DIVIDE(CurrentMonth-LastMonth,LastMonth)
        RETURN

        IF(LastMonth=0,0,VarTrend)

- Step 7: New measure was added to the previous chart.

Sanp of the chart,

> ![Line_Chart](https://github.com/user-attachments/assets/89af9fb9-433f-4f68-bfde-61bafb11e86e)

- Step 8: Three card visuals were added:

(1) First one to represent Total Revenue of the year. 

(2) Second one to represent Total Revenue of the last month registered. Following DAX expression was written:

        TotalRevenueByLastMonth = CALCULATE(SUM('Online Sales Data'[Total Revenue]),'Calendar Table'[MonthNum]=MAX('Calendar Table'[MonthNum]))

(3) Last one to represent % of difference between the last month and the previous one:

        %CurrentMonth = CALCULATE([TrendOverMonth],'Calendar Table'[MonthNum]=MAX('Calendar Table'[MonthNum]))

       (3.1) New measure also was added to represent in green or red color the %CurrentMonth value:

        CurrentMonth_Color = IF([%CurrentMonth] >= 0, "Green", "Red")
        
Snap of the card visuals,

> ![Card Visuals](https://github.com/user-attachments/assets/3775e78b-fe36-4d57-abd4-9301efeec14c)

- Step 9: Visual Filters (Silcers) and a Donut chart were added to explore the % of Total Revenue by Payment method to investigate the impact of payment methods on sales volume or revenue.

Snap of the chart,

> ![Total Revenue By Payment Method](https://github.com/user-attachments/assets/f5d459ea-6d61-424f-9734-13d89f49d48d)

- Step 10: A Clustered bar chart was added to represent Total Units Sold by product category.

> ![Units Sold By Category](https://github.com/user-attachments/assets/d95707fe-249e-47e0-b8c7-dddd1332942e)

- Step 11: A Top 8 table was added to identify top-selling products within each category to optimize inventory and marketing strategies.

> ![Top 8 table](https://github.com/user-attachments/assets/878d35e2-34d6-44e9-b91d-a06f2efa0ab4)

- Step 12 : The report was then published to Power BI Service.

> ![Publishing to Power BI](https://github.com/user-attachments/assets/791e9d3a-e28f-44f2-ad7a-ea44edb32bfb)

# Snapshot of Dashboard (Power BI Service)

> ![Online Sales Data Dashboard](https://github.com/user-attachments/assets/8a649329-6fdd-48ff-b07a-17600e3620e1)

# Insights

A single page report was created on Power BI Desktop & it was then published to Power BI Service.

Following inferences can be drawn from the dashboard:

### [1] Total Revenue of the year = $80,568 USD

(1.1) January, February, March, and April stood out as the best months of the year, accounting for 62.86% of total sales.

(1.2) Last three months were the lowest performing of the year, representing 26.63% of total sales."

(1.3) Credit card is the preferred payment method, accounting for 63.51% of total sales.

(1.4) 45.73% of the total sales were made by individuals residing in North America.

(1.5) The clothing category is highlighted as the top-performing category, with a total of 145 items sold, representing 10% of total sales."

### [2] Total Revenue of the year by North America = $36,844 USD

(2.1) January stood out as the best month of the year, accounting for 22.55% of total sales in Norh America.

(2.2) July was the lowest performing of the year, representing 5.89% of total sales."

(2.3) All individuals from North America exclusively preferred to make purchases using a credit card as the payment method.

(2.4) People from North America prefer to spend their money on books and electronics.
