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

> ![Total Revenue By Payment Method](https://private-user-images.githubusercontent.com/168421028/349629538-19eaa7e8-0d4f-4667-b8cc-6027d1b80e8e.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MjEyNDA4MzcsIm5iZiI6MTcyMTI0MDUzNywicGF0aCI6Ii8xNjg0MjEwMjgvMzQ5NjI5NTM4LTE5ZWFhN2U4LTBkNGYtNDY2Ny1iOGNjLTYwMjdkMWI4MGU4ZS5wbmc_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjQwNzE3JTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI0MDcxN1QxODIyMTdaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT01NTZiNTVjMDgyYTJlYmY0ZWJjNDBkMzBmODY0M2EwYTkzNTExN2E4ZGRhZGZmZTkwZjgxMzZiNDMwZDNhMGFjJlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCZhY3Rvcl9pZD0wJmtleV9pZD0wJnJlcG9faWQ9MCJ9.RlgePspEXio810eUz_H9D2QrDDpltv2WF_Qi29Oh4NM&_sm_au_=iVVZFSDkksNNtvTML321jK0f1JH33)

- Step 10: A Clustered bar chart was added to represent Total Units Sold by product category.

> ![Units Sold By Category](https://private-user-images.githubusercontent.com/168421028/349630929-e2dd9813-5cf4-4c8d-ae2c-0731f6b1152b.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MjEyNDExMzUsIm5iZiI6MTcyMTI0MDgzNSwicGF0aCI6Ii8xNjg0MjEwMjgvMzQ5NjMwOTI5LWUyZGQ5ODEzLTVjZjQtNGM4ZC1hZTJjLTA3MzFmNmIxMTUyYi5wbmc_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjQwNzE3JTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI0MDcxN1QxODI3MTVaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT0yNTY5OTM0NDM0OTBjOThmMjJiZDZlMmI4N2Y5YjIyZDlmMmJhN2U5NjdjNTY3ZDk4ODgxMDM4YjFjYTE4NjI1JlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCZhY3Rvcl9pZD0wJmtleV9pZD0wJnJlcG9faWQ9MCJ9.uXiQuhX9Tn0tZ_RuIU9J7kwg9gubf_bzBMC5_k3Mb3U&_sm_au_=iVVZFSDkksNNtvTML321jK0f1JH33)

- Step 11: A Top 8 table was added to identify top-selling products within each category to optimize inventory and marketing strategies.

> ![Top 8 table](https://private-user-images.githubusercontent.com/168421028/349632289-81c827c1-315e-4510-b129-f4d9a444b969.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MjEyNDE0MzIsIm5iZiI6MTcyMTI0MTEzMiwicGF0aCI6Ii8xNjg0MjEwMjgvMzQ5NjMyMjg5LTgxYzgyN2MxLTMxNWUtNDUxMC1iMTI5LWY0ZDlhNDQ0Yjk2OS5wbmc_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjQwNzE3JTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI0MDcxN1QxODMyMTJaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT00MzcxMzIwYzk4ZmY4NDZhODIyNWUxMDQyYjgwMjBiNDI3YTVkNWRmMjIwNjlkNzc3YzA4ZWNjMzczNjk3ZDI1JlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCZhY3Rvcl9pZD0wJmtleV9pZD0wJnJlcG9faWQ9MCJ9.zDxw_Te7U31o7D3XIiSGWw28PGfjF83koGCKrA4PItY&_sm_au_=iVVZFSDkksNNtvTML321jK0f1JH33)

- Step 12 : The report was then published to Power BI Service.

> ![Publishing to Power BI](https://private-user-images.githubusercontent.com/168421028/349633460-1f10cb08-74ef-40ec-83e5-cc82ba498151.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MjEyNDE2NzIsIm5iZiI6MTcyMTI0MTM3MiwicGF0aCI6Ii8xNjg0MjEwMjgvMzQ5NjMzNDYwLTFmMTBjYjA4LTc0ZWYtNDBlYy04M2U1LWNjODJiYTQ5ODE1MS5wbmc_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjQwNzE3JTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI0MDcxN1QxODM2MTJaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT1iZjQ4NjAzZTBjN2Q3OGI4OWUyZjQxOTc1NzZkMzZjZjBhNTZmZjUzYTgzOTU5YjFkYWM3MmNmNjY4ZmQxODg5JlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCZhY3Rvcl9pZD0wJmtleV9pZD0wJnJlcG9faWQ9MCJ9.ai3kXYBRXTewhMyzRBPSCPvQlDMW1z2cqoXGAwk7mw8&_sm_au_=iVVZFSDkksNNtvTML321jK0f1JH33)

# Snapshot of Dashboard (Power BI Service)

> ![Online Sales Data Dashboard](https://private-user-images.githubusercontent.com/168421028/349647196-06414519-8946-4320-804f-71b4a850fc6d.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MjEyNDQ3NjUsIm5iZiI6MTcyMTI0NDQ2NSwicGF0aCI6Ii8xNjg0MjEwMjgvMzQ5NjQ3MTk2LTA2NDE0NTE5LTg5NDYtNDMyMC04MDRmLTcxYjRhODUwZmM2ZC5wbmc_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjQwNzE3JTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI0MDcxN1QxOTI3NDVaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT1kNWZlNDk4NmQwMTQ4NGFjNTg2N2ZlY2E5MDIzNDI2ZTVmMzAxOWY2NzU3NDZmMTM0NzQyMTUwOTgyMjdhYTNmJlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCZhY3Rvcl9pZD0wJmtleV9pZD0wJnJlcG9faWQ9MCJ9.JNm1T4pahVNRRI-z0B4LnUWu0NajVhsIZ968vIqbNLE&_sm_au_=iVVGHrr7WKK7v5PQL321jK0f1JH33)

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
