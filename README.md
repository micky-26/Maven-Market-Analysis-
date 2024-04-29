# Maven-Market-Analysis

![Dashboard](https://github.com/micky-26/Maven-Market-Analysis-/assets/106061980/ffb43ad6-1480-4e6f-998d-87613ef6fe05)


***INTRODUCTION:*** 

Maven Market Analysis is a comprehensive Power BI project designed to provide insightful analysis and visualization of sales data. With a dataset comprising **10,281 records** across seven different tables, including Calendar, Customers, Products, Regions, Return Data, Stores, and Transaction Data, this project offers a robust foundation for understanding sales performance.

***PROJECT WORKFLOW:***
The project workflow encompasses several key steps.
1. It begins with connecting to the data source and shaping the data to ensure its suitability for analysis.
2. Subsequently, a data model is created to facilitate efficient querying and visualization.
3. **DAX** measures are then added to calculate important key performance indicators **(KPIs)**, such as current and previous month transactions, profits, and returns.

***STEPS PERFORMED***

**PART I - Connecting and Shaping Data**

Loading MavenMarket_Customers CSV file 

1. Added a new column named "full_name" to merge the "first_name" and "last_name" columns, separated by a space.
   Open Power Query Editor > Selected first_name and last_name > Go to Merge Columns.
2. Created a new column named "birth_year" to extract the year from the "birthdate" column, and format it as text
   Selected Dob column in Power Query Editor > Go to Date & Time Section > Select Age > Right click on newly created Age column > Click on Transform > Total Years.
   Then Round Down the Age column to get the exact Age.
3. Created a conditional column named "has_children" which equals "N" if "total_children" = 0, otherwise "Y".

Loading MavenMarket_Products CSV file

1. Added a calculated column named "discount_price", equal to 90% of the original retail price
   Got to Add Column > Custom Column . 
   discount_price = product_retail_price * 0.9
2. Replaced "null" values with zeros in both the "recyclable" and "low-fat" columns.
   Selected recyclable column > Go to Replace values under Transform > Enter "null" in Value to Find and  "0" in Replace With.

Loading MavenMarket_Stores CSV file

1. Added a calculated column named "full_address", by merging "store_city", "store_state", and "store_country". 
   Select store_city, store_state, and store_country > Go To Merge Columns .
2. Added a calculated column named "area_code", by extracting the characters before the dash ("-") in the "store_phone" field.
   Select Sore_Phone Column > Go To Add Column > Choose Extract under From Text > Text Before Delimiter > Add "-" in Delimiter > OK

Loading MavenMarket_Regions CSV file

1. Named the table "Regions" and made sure that headers have been promoted
2. Confirmed that data types are accurate (Note: "region_id" should be whole numbers)

Loading MavenMarket_Calendar CSV file

1. Named the table "Calendar" and made sure that headers have been promoted
2. Used the date tools  under Add Column in the query editor to add the following columns:
      Start of Week (starting Sunday
      Name of Day
      Start of Month
      Name of Month
      Quarter of Year
      Year

Loading MavenMarket_Returns CSV file

1. Named the table "Return_Data" and made sure that headers have been promoted
2. Confirmed that data types are accurate (all ID columns and quantity should be whole numbers)

loading MavenMarket_Transaction CSV File


1. Added a new folder named "MavenMarket Transactions", containing both the MavenMarket_Transactions_1997 and MavenMarket_Transactions_1998 csv files
2. Connected to the folder path, and choose Combine $ Transform.
3. Click the "Content" column header (double arrow icon) to combine the files, then remove the "Source.Name" column
4. Named the table "Transaction_Data", and confirmed that headers have been promoted
5. Confirmed that data types are accurate (all ID columns and quantity should be whole numbers)


**PART II - Creating the Data Model**

1. Connected Transaction_Data to Customers, Products, and Stores using valid primary/foreign keys 
2. Connected Transaction_Data to Calendar using both date fields, with an inactive "stock_date" relationship
3. Connected Return_Data to Products, Calendar, and Stores using valid primary/foreign keys
4. Connected Stores to Regions as a "snowflake" schema
5. Confirmed all relationships follow one-to-many cardinality, with primary keys (1) on the lookup side and foreign keys (*) on the data side.
6. Updated all date fields (across all tables) to the "M/d/yyyy" format using the formatting tools in the Modeling tab
7. Updated "product_retail_price", "product_cost", and "discount_price" to Currency ($ English) format
8. In the Customers table, categorized "customer_city" as City, "customer_postal_code" as Postal Code, and "customer_country" as Country/Region
9. In the Stores table, categorized "store_city" as City, "store_state" as State or Province, "store_country" as Country/Region, and "full_address" as Address 


**PART III - Adding DAX Measures**

1. In the Calendar table, add a column named "Weekend"
   Equals "Y" for Saturdays or Sundays (otherwise "N")

   Weekend = IF('Calendar'[Day Name] = "Saturday" || 'Calendar'[Day Name] = "Sunday", "Y", "N")
   
2. In the Calendar table, add a column named "End of Month"
   Returns the last date of the current month for each row

   End of Month = EOMONTH('Calendar'[Date], 0)

3. In the Customers table, add a column named "Current Age"
   Calculates current customer ages using the "birthdate" column and the TODAY() function

   Current Age = 
    DATEDIFF('Customers'[birthdate], TODAY(), YEAR)

4. In the Customers table, add a column named "Priority"
   Equals "High" for customers who own homes and have Golden membership cards (otherwise "Standard")

   Priority = IF('Customers'[homeowner]= "Y" && 'Customers'[member_card] = "Golden", "High", "Standard")

5. In the Customers table, add a column named "Short_Country"
   Returns the first three characters of the customer country, and converts to all uppercase

   Short_Country = UPPER(LEFT('Customers'[customer_country], 3))

6. In the Customers table, add a column named "House Number"
   Extract all characters/numbers before the first space in the "customer_address" column

   House Number = LEFT('Customers'[customer_address], FIND(" ", 'Customers'[customer_address]) - 1)

7. In the Products table, add a column named "Price_Tier"
   Equals "High" if the retail price is >$3, "Mid" if the retail price is >$1, and "Low" otherwise

   Price_Tier = IF('Products'[product_retail_price]> 3, "High",
        IF('Products'[product_retail_price] > 1, "Mid", "Low"))

8. In the Stores table, add a column named "Years_Since_Remodel"
   Calculate the number of years between the current date (TODAY()) and the last remodel date

   Years_Since_Remodel = DATEDIFF('Stores'[last_remodel_date], TODAY(), YEAR)

In the REPORT view, add the following measures:

1. Create new measures named "Quantity Sold" and "Quantity Returned" to calculate the sum of quantity from each data table

   Quantity Sold = SUM('Sales'[quantity])
   Quantity Returned = SUM('Returns'[quantity])

2. Create new measures named "Total Transactions" and "Total Returns" to calculate the count of rows from each data table

   Total Transactions = COUNTROWS('Transaction_Data')
   Total Returns = COUNTROWS('Returns')
   
3. Create a new measure named "Return Rate" to calculate the ratio of quantity returned to quantity sold (format as %)

   Return Rate = 
   DIVIDE(
    [Quantity Returned], 
    [Quantity Sold],
    0
   ) * 100

4. Create a new measure named "Weekend Transactions" to calculate transactions on weekends

   Weekend Transactions = 
   CALCULATE(
    [Total Transactions],
    FILTER(
        'Calendar',
        'Calendar'[Day Name] = "Saturday" || 'Calendar'[Day Name] = "Sunday"
    )
   )

5. Create a new measure named "% Weekend Transactions" to calculate weekend transactions as a percentage of total transactions (format as %)

   % Weekend Transactions = 
   DIVIDE(
    [Weekend Transactions],
    [Total Transactions],
    0
   ) * 100

6. Create a new measure to calculate "Total Revenue" based on transaction quantity and product retail price

   Total Revenue = 
   SUMX(
    'Transaction_data',
    'Transaction_data'[Quantity] * RELATED('Products'[product_retail_price])
   )

7. Create a new measure to calculate "Total Cost" based on transaction quantity and product cost

   Total Cost = SUMX('Transaction_data', 'Transaction_Data'[quantity] * RELATED('Products'[product_cost]))

8. Create a new measure named "Total Profit" to calculate total revenue minus total cost,

   Total Profit = [Total Revenue] - [Total Cost]

9. Create a new measure to calculate "Profit Margin" by dividing total profit by total revenue calculate total revenue

    Profit Margin = DIVIDE([Total Profit], [Total Revenue])

10. Create a new measure named "Unique Products" to calculate the number of unique product names in the Products table

     Unique Products = DISTINCTCOUNT('Products'[product_name])
    
12. Create a new measure named "YTD Revenue" to calculate year-to-date total revenue

   YTD Revenue = TOTALYTD([Total Revenue], 'Calendar'[date])

13. Create a new measure named "60-Day Revenue" to calculate a running revenue total over a 60-day period

    60-Day Revenue = TOTALMTD([Total Revenue], DATEADD('Calendar'[date], -59, DAY))

14. Create new measures named  "Last Month Transactions", "Last Month Revenue", "Last Month Profit", and "Last Month Returns"

    Last Month Transactions = CALCULATE([Total Transactions], PREVIOUSMONTH('Calendar'[date]))
    Last Month Revenue = CALCULATE([Total Revenue], PREVIOUSMONTH('Calendar'[date]))
    Last Month Profit = CALCULATE([Total Profit], PREVIOUSMONTH('Calendar'[date]))
    Last Month Returns = CALCULATE([Total Returns], PREVIOUSMONTH('Calendar'[date]))

15. Create a new measure named "Revenue Target" based on a 5% lift over the previous month revenue

   Revenue Target = [Last Month Revenue] * 1.05

***Key Takeaway***
1. Current Month's Transaction is **5.69%** more than the Previous Month's Transaction.
2. Current Month's Profit is **5.61%** more than the Previous Month's Profit.
3. Current Month's Returns is **2.9%** less than the Previous Month's Returns.
4. The USA has the highest transaction of $83,986.
5. The store has crossed the revenue target of **$1,19,477** by generating a Total Revenue of **$1,20,161**.
6. Hermanos Product brand has brought the highest Total Transaction of **$5,542** and the highest profit of **$21,753**. Carrington has the lowest Return rate of **0.78%**.
    
