# Maven-Market-Analysis-
PART I - Connecting and Shaping Data

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

