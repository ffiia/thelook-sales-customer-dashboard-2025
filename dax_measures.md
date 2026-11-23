## Page 1: Sales Performance
1. TOTAL REVENUE = calculate(sum(sales_data[sale_price]), sales_data[item_status]= "Complete", sales_data[order_date]>= DATE(2025,1,1), sales_data[order_date]< DATE(2026,1,1))
2. REVENUE GROWTH YoY = FORMAT([REVENUE GROWTH],"+0.00%;-0.00%;0.00%")&UNICHAR(13)&UNICHAR(10)&[Revenue Growth Status]
3. TOTAL ORDERS = calculate(DISTINCTCOUNT(sales_data[order_id]),sales_data[item_status]="Complete",YEAR(sales_data[order_date])=2025)
4. Average Order Value = CALCULATE(DIVIDE(SUM(sales_data[sale_price]), DISTINCTCOUNT(sales_data[order_id])),sales_data[order_status]="Complete", YEAR(sales_data[order_date]) = 2025)
5. CANCELLED RATE = DIVIDE(CALCULATE(COUNTROWS(sales_data),sales_data[item_status] = "Cancelled",YEAR(sales_data[order_created_at]) = 2025),CALCULATE(COUNTROWS(sales_data),YEAR(sales_data[order_created_at]) = 2025),0)
6. RETURN RATE = DIVIDE(CALCULATE(COUNTROWS(sales_data),sales_data[item_status]= "Returned",YEAR(sales_data[order_created_at])= 2025), CALCULATE(COUNTROWS(sales_data),YEAR(sales_data[order_created_at]) = 2025),0)
7. Net Realized Revenue vs Lost Revenue:
   Month Name = FORMAT(sales_data[order_created_at],"MMM")
   Net realized revenue 2025 = CALCULATE(SUM(sales_data[sale_price]),sales_data[item_status]="Complete",YEAR(sales_data[order_created_at])=2025)
   Lost revenue 2025 = CALCULATE(SUM(sales_data[sale_price]),sales_data[item_status] IN {"Cancelled","Returned"},YEAR(sales_data[order_created_at])=2025)
8. Monthly Customers = DISTINCTCOUNT(sales_data[user_id])
9. Revenue 2025 = CALCULATE(SUM(sales_data[sale_price]),sales_data[order_date]>=DATE(2025,1,1),sales_data[order_date]<DATE(2026,1,1))

## Page 2: Customer Demographic & Behanior
1. Total Realized Revenue = CALCULATE(SUM(sales_data[sale_price]),sales_data[item_status]="Complete", sales_data[order_date]>= DATE(2025,1,1), sales_data[order_date]< DATE(2026,1,1))
2. Customer Growth YoY = VAR Cust2025=[customers 2025] VAR Cust2024=[customers 2024] RETURN DIVIDE(cust2025-Cust2024, Cust2024)
3. No. of Customers = CALCULATE(DISTINCTCOUNT(sales_data[user_id]),sales_data[order_status]="Complete",YEAR(sales_data[order_date])=2025)
4. Repeat Customer Rate = VAR customerorders= CALCULATETABLE(SUMMARIZE(FILTER(sales_data,sales_data[item_status]="Complete" && YEAR(sales_data[order_date])=2025),sales_data[user_id],"OrderCount", DISTINCTCOUNT(sales_data[order_id]))) VAR RepeatCustomers=COUNTROWS(FILTER(customerorders,[OrderCount]>1)) RETURN DIVIDE(RepeatCustomers,[No. of Customers])
5. Order per Customer now = VAR orders2025 = CALCULATE(DISTINCTCOUNT(sales_data[order_id]),sales_data[item_status] = "Complete", YEAR(sales_data[order_date])=2025) VAR customers2025 =CALCULATE(DISTINCTCOUNT(sales_data[user_id]), sales_data[item_status] = "Complete",FILTER(ALL(sales_data[order_date]),YEAR(sales_data[order_date])=2025)) RETURN DIVIDE(orders2025, customers2025)
6. Buyers 2025 = CALCULATE(DISTINCTCOUNT(sales_data[user_id]),sales_data[item_status]="Complete",YEAR(sales_data[order_date])=2025)
7. Gender Label = SWITCH(sales_data[gender],"F","Female","M","Male",sales_data[gender])
8. Monthly Customer Count = COUNTROWS('Customer Monthly Summary')
   Customer Monthly Summary = VAR monthlybase=SUMMARIZE(FILTER(sales_data,sales_data[order_date]>=DATE(2025,1,1)&&sales_data[order_date]<DATE(2026,1,1)),sales_data[user_id],sales_data[year],sales_data[month],sales_data[month_name],"monthlyorders",DISTINCTCOUNT(sales_data[order_id])) RETURN ADDCOLUMNS(monthlybase,"firstorderdate", VAR thisuser=[user_id] RETURN CALCULATE(MIN(sales_data[order_date]),FILTER(ALL(sales_data),sales_data[user_id]=thisuser)))
   Customer Type = VAR MonthStart =DATE('Customer Monthly Summary'[year],'Customer Monthly Summary'[month], 1)VAR FirstOrder ='Customer Monthly Summary'[firstorderdate] RETURN IF(FirstOrder >= MonthStart && FirstOrder < EOMONTH(MonthStart, 0) + 1, "New Customers", "Returning Customers").
9. Age Group = SWITCH(TRUE(),sales_data[age]<18, "Under 18",sales_data[age]<=24, "18-24", sales_data[age]<=34, "25-34", sales_data[age]<=44, "35-44", sales_data[age]<=54, "45-54", "55+")
   Total Items Sold 2025 = CALCULATE(COUNT(sales_data[order_item_id]), sales_data[item_status]="Complete",YEAR(sales_data[order_created_at])=2025)
10. Order per Customer 2025 = VAR orders2025 =CALCULATE(DISTINCTCOUNT(sales_data[order_id]),sales_data[item_status] = "Complete", FILTER(ALL(sales_data[order_created_at]), YEAR(sales_data[order_created_at])=2025)) VAR customers2025 =CALCULATE(DISTINCTCOUNT(sales_data[user_id]), sales_data[item_status] = "Complete",FILTER(ALL(sales_data[order_created_at]),YEAR(sales_data[order_created_at])=2025)) RETURN DIVIDE(orders2025, customers2025)
    Revenue per Customer 2025 = VAR sales2025=CALCULATE(SUM(sales_data[sale_price]),sales_data[item_status]="Complete",YEAR(sales_data[order_created_at])=2025) VAR customers2025=CALCULATE(DISTINCTCOUNT(sales_data[user_id]),sales_data[item_status]="Complete",YEAR(sales_data[order_created_at])=2025) RETURN DIVIDE(sales2025,customers2025)
