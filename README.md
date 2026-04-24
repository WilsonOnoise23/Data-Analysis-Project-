E-COMMERCE SALES DATA ANALYSIS AND 
DASHBOARD DEVELOPMENT 
BY 
WILSON ONOISE OIGIANGBE 
MENTOR: MISS TOLUDOYIN SHOPEIN 
GROUP NAME: 3MTT C2 ADVANCED DATA 
ANALYSIS 102 
INTRODUCTION 
This project focuses on analyzing an e-commerce dataset to extract meaningful insights that support 
business decision-making. The dataset initially contained inconsistencies, missing values, and formatting 
issues, which required thorough data cleaning before analysis. 
OBJECTIVES 
The main objectives of this project are: 
 To transform the e-commerce dataset into a clean, structured format. 
 To extract actionable business insights that can improve revenue, customer retention and 
operational efficiency.  
 To analyze sales performance across different dimensions (City, Age, Time). 
 To identify key business trends and patterns.  
 To build an interactive dashboard for decision-making. 
Key Business Questions include: 
 Which regions generate the most revenue? 
 What product categories perform best? 
 What are the peak sales periods? 
 How do discounts affect revenue? 
 What customer segments drive the most sales? 
DATASET DESCRIPTION 
The dataset contains transactional data including: 
 Order details (order_id, order_date, order_time)  
 Customer information (customer_id, customer_age)  
 Product details (product_id, category)  
 Sales data (quantity, unit_price, discount_pct)  
 Location (region, shipping_city)  
 Additional fields (rating, return_flag, payment_method) 
DATA CLEANING PROCESS 
This was the most critical phase of the project. 
Handling Missing Values 
 Filled categorical nulls with “Unknown”  
 Imputed numerical values using mean/median  
Fixing Data Types 
 Converted unit_price from text to numeric  
 Combined order_date and order_time into a datetime column  
Handling Inconsistent Data 
 Standardized text (e.g., city names, categories)  
 Corrected invalid age values:  
o -5 → 50  
o 150 → 15  
o 200 → 20  
Removing Duplicates 
 Identified duplicate order_id values  
 Retained the most relevant records 
FEATURE ENGINEERING 
New features were created to enhance analysis: 
 Revenue Column: 
Revenue = Quantity × Unit Price × (1 - Discount%)  
 Datetime Features: 
Year, Month, Day, Hour  
 Customer Segmentation: 
Age groups (Teen, Young Adult, Adult, Senior, etc.)  
M CODE (POWER QUERY) 
 Source: = 
Csv.Document(File.Contents("C:\Users\DELL\Documents\ecommerce.csv"),[Delimiter=",", 
Columns=24, Encoding=1252, QuoteStyle=QuoteStyle.None]) 
 Promote Headers: = Table.PromoteHeaders(Source, [PromoteAllScalars=true]) 
 Changed Type: = Table.TransformColumnTypes(#"Promoted Headers",{{"order_id", type text}, 
{"customer_id", type text}, {"product_id", type text}, {"order_date", type text}, {"order_time", type 
time}, {"region", type text}, {"category", type text}, {"quantity", Int64.Type}, {"unit_price", type 
number}, {"discount_pct", type number}, {"payment_method", type text}, {"customer_age", 
Int64.Type}, {"rating", type number}, {"return_flag", type text}, {"shipping_city", type text}, 
{"order_date_clean", type date}, {"order_datetime", type datetime}, {"year", Int64.Type}, {"month", 
Int64.Type}, {"day", Int64.Type}, {"hour", Int64.Type}, {"normalized_return_flag", Int64.Type}, 
{"revenue", type number}, {"age_group", type text}}) 
 Removed Column: = Table.RemoveColumns(#"Changed Type",{"order_date"}) 
 Inserted Year: = Table.AddColumn(#"Removed Columns", "Year.1", each 
Date.Year([order_date_clean]), Int64.Type) 
 Removed Columns1: = Table.RemoveColumns(#"Inserted Year",{"year"}) 
 Inserted Month: = Table.AddColumn(#"Removed Columns1", "Month.1", each 
Date.Month([order_date_clean]), Int64.Type) 
 Inserted Start of Month: = Table.AddColumn(#"Inserted Month", "Start of Month", each 
Date.StartOfMonth([order_date_clean]), type date) 
 Inserted Day Name: = Table.AddColumn(#"Inserted Start of Month", "Day Name", each 
Date.DayOfWeekName([order_date_clean]), type text) 
 Removed Columns2: = Table.RemoveColumns(#"Inserted Day Name",{"Month.1", "Start of 
Month"}) 
 Inserted Month Name: = Table.AddColumn(#"Removed Columns2", "Month Name", each 
Date.MonthName([order_date_clean]), type text) 
 Reordered Columns: = Table.ReorderColumns(#"Inserted Month Name",{"order_id", 
"customer_id", "product_id", "order_time", "region", "category", "quantity", "unit_price", 
"discount_pct", "payment_method", "customer_age", "rating", "return_flag", "shipping_city", 
"order_date_clean", "order_datetime", "Year.1", "month", "Month Name", "day", "Day Name", 
"hour", "normalized_return_flag", "revenue", "age_group"}) 
 Changed Type1: = Table.TransformColumnTypes(#"Reordered Columns",{{"revenue", 
Currency.Type}, {"unit_price", Currency.Type}}) 
 Renamed Columns: = Table.RenameColumns(#"Changed Type1",{{"order_id", "Order_ID"}, 
{"customer_id", "Customer_ID"}, {"product_id", "Product_ID"}, {"order_time", "Order_Time"}, 
{"region", "Region"}, {"category", "Category"}, {"quantity", "Quantity"}, {"unit_price", "Unit_Price"}, 
{"discount_pct", "Discount_Pct"}, {"payment_method", "Payment_Method"}, {"customer_age", 
"Customer_Age"}, {"rating", "Rating"}, {"return_flag", "Return_Flag"}, {"shipping_city", 
"Shipping_City"}, {"order_date_clean", "Order_Date"}, {"order_datetime", "Order_Datetime"}, 
{"Year.1", "Year"}, {"month", "Month"}, {"day", "Day"}, {"hour", "Hour"}, {"normalized_return_flag", 
"Normalized_Return_Flag"}, {"revenue", "Revenue"}, {"age_group", "Age_Group"}}) 
 Filtered Rows: = Table.SelectRows(#"Renamed Columns", each [Age_Group] <> null and 
[Age_Group] <> "") 
 Replaced Values …. 21: = Table.ReplaceValue(#"Replaced 
Value…20","Beautty…Tois","Beauty…Toys",Replacer.ReplaceText,{"Category"}) 
 Filtered Rows1: = Table.SelectRows(#"Replaced Value21", each true) 
 Trimmed Text: = Table.TransformColumns(#"Filtered Rows1",{{"Shipping_City", Text.Trim, type 
text}}) 
 Filtered Rows2: = Table.SelectRows(#"Trimmed Text", each true) 
 Inserted Start of Month1: = Table.AddColumn(#"Filtered Rows2", "Start of Month", each 
Date.StartOfMonth([Order_Date]), type date) 
 Inserted Start of Year: = Table.AddColumn(#"Filtered Rows2", "Start of Month", each 
Date.StartOfMonth([Order_Date]), type date) 
 Inserted Start of Week: = Table.AddColumn(#"Inserted Start of Year", "Start of Week", each 
Date.StartOfWeek([Order_Date]), type date) 
 Removed Columns3: = Table.RemoveColumns(#"Inserted Start of Week",{"Year", "Month", 
"Month Name", "Day", "Day Name", "Hour"}) 
 Added Custom: = Table.AddColumn(#"Removed Columns3", "Actual Revenue", each [Quantity] * 
[Unit_Price]) 
 Reorderd Columns1: = Table.ReorderColumns(#"Added Custom",{"Order_ID", "Customer_ID", 
"Product_ID", "Order_Time", "Region", "Category", "Quantity", "Unit_Price", "Discount_Pct", 
"Actual Revenue", "Revenue", "Payment_Method", "Customer_Age", "Rating", "Return_Flag", 
"Shipping_City", "Order_Date", "Order_Datetime", "Normalized_Return_Flag", "Age_Group", 
"Start of Month", "Start of Year", "Start of Week"}) 
 Changed Type2: = Table.TransformColumnTypes(#"Reordered Columns1",{{"Actual Revenue", 
Currency.Type}}) 
 Renamed Columns1: = Table.RenameColumns(#"Changed Type2",{{"Discount_Pct", 
"Discount_Percentage"}}) 
 Sorted Rows: = Table.Sort(#"Renamed Columns1",{{"Discount_Percentage", Order.Ascending}}) 
 Added Conditional Column: = Table.AddColumn(#"Sorted Rows", "Discount Category", each if 
[Discount_Percentage] <= 10 then "Very Low Discount" else if [Discount_Percentage] <= 20 then 
"Low Discount" else if [Discount_Percentage] <= 30 then "High Discount" else if 
[Discount_Percentage] <= 40 then "Very High Discount" else null) 
 Reordered Column2: = Table.ReorderColumns(#"Added Conditional Column",{"Order_ID", 
"Customer_ID", "Product_ID", "Order_Time", "Region", "Category", "Quantity", "Unit_Price", 
"Discount_Percentage", "Discount Category", "Actual Revenue", "Revenue", "Payment_Method", 
"Customer_Age", "Rating", "Return_Flag", "Shipping_City", "Order_Date", "Order_Datetime", 
"Normalized_Return_Flag", "Age_Group", "Start of Month", "Start of Year", "Start of Week"}) 
 Changed Type3: = Table.TransformColumnTypes(#"Reordered Columns2",{{"Discount 
Category", type text}}) 
DATA MODELLING 
 Order Quantity = SUM('E-commerce'[Quantity]) 
 Return Rate = DIVIDE(SUM('E-commerce'[Normalized_Return_Flag]), COUNT('E
commerce'[Normalized_Return_Flag])) 
 Total Acutal Revenue = SUM('E-commerce'[Actual Revenue]) 
 Total Customers = DISTINCTCOUNT('E-commerce'[Customer_ID]) 
 Total Orders = DISTINCTCOUNT('E-commerce'[Order_ID]) 
 Total Products = DISTINCTCOUNT('E-commerce'[Product_ID]) 
 Total Returns = SUM('E-commerce'[Normalized_Return_Flag]) 
 Total Revenue = SUM('E-commerce'[Revenue]) 
EXPLORATORY DATA ANALYSIS 
The dataset was analyzed to uncover trends and patterns: 
 Revenue distribution by City and Product Category  
 Customer behavior based on age groups  
 Time-based sales trends  
 Impact of discounts on revenue 
DASHBOARD DEVELOPMENT 
An interactive Power BI dashboard was created featuring: 
 KPI Cards (Revenue, Orders, Returns )  
 Sales Performance Charts  
 Customer Insights Visuals  
 Time-Based Analysis  
 Product Performance Metrics 
 
 
 
 
KEY INSIGHTS 
 The regions had relatively equal contributions to the overall revenue  
 The Shipping_cities generated relatively equal revenue 
 Electronic product category drove the highest sales revenue  
 Peak sales occurred around 2:04:31 PM 
 The Middle Age, Young Adult and Adult Age_Groups contributed significantly to the overall 
revenue 
 Products with Low Discounts drove the highest revenue  
 Electronic products category had the highest return rates 
BUSINESS RECOMMENDATIONS 
 Optimise marketing efforts across the regions 
 Assign Very Low discounts for products to increase revenue 
 Improve quality control for high-return products such electronics and clothing  
 Target high-value customer age groups such as middle age, young adult and adults 
CHALLENGES FACED 
 Mixed date formats across the dataset  
 Incorrect and unrealistic age values  
 Duplicate records affecting data accuracy 
FUTURE IMPROVEMENTS 
 Implement predictive analytics (sales forecasting)  
 Integrate real-time data pipelines  
 Enhance dashboard interactivity 
TOOLS & TECHNOLOGIES 
 Python (Pandas)  
 Power BI  
 Excel  
 DAX  
CONCLUSION 
This project demonstrates the importance of data cleaning, analysis, and visualization in transforming raw 
data into actionable insights. The final dashboard provides a clear and interactive way for businesses to 
monitor performance and make informed decisions. 
