# Product Analytics Dashboard for a Bike Store

## Table of Contents

- [Project Overview](#project-overview)
- [Data Sources](#data-sources)
- [Tools](#tools)
- [Data Preparation](#data-preparation)
- [Exploratory Data Analysis](#exploratory-data-analysis)
- [Data Analysis](#data-analysis)
- [Data Visualization](#data-visualization)
- [Key Findings](#key-findings)
- [Recommendations](#recommendations)
- [Limitations](#limitations)
- [Next Steps](#next-steps)
- [Resources](#resources)


### Project Overview

The objective of this project is to provide insights into the product sales of an e-commerce company. From these insights, I will gain a deeper understanding of the company's sales performance, which will assist me in identifying trends and making data-driven recommendations.

### Data Sources

*This is the sample database from [sqlservertutorial.net](https://www.sqlservertutorial.net/).*

- Brands Data: The brands table stores the brand’s information of bikes, for example, Electra, Haro, and Heller.
- Categories Data: The categories table stores the bike’s categories such as children bicycles, comfort bicycles, and electric bikes.
- Order Items: The 'order_items' table stores the line items of a sales order. Each line item belongs to a sales order specified by the order_id column.
- Orders Data: The orders table stores the sales order’s header information including customer, order status, order date, required date, shipped date. It also stores the information on where the sales transaction was created (store) and who created it (staff).
- Products Data: The products table stores the product’s information such as name, brand, category, model year, and list price. Each product belongs to a brand specified by the brand_id column. Hence, a brand may have zero or many products. Each product also belongs a category specified by the category_id column. Also, each category may have zero or many products.
- Staffs Data: The staffs table stores the essential information of staffs including first name, last name. It also contains the communication information such as email and phone. A staff works at a store specified by the value in the store_id column. A store can have one or more staffs. A staff reports to a store manager specified by the value in the manager_id column. If the value in the manager_id is null, then the staff is the top manager. If a staff no longer works for any stores, the value in the active column is set to zero.
It also stores the information on where the sales transaction was created (store).
- Stocks Data: The stocks table stores the inventory information i.e. the quantity of a particular product in a specific store.
- Stores Data: The stores table includes the store’s information. Each store has a store name, contact information such as phone and email, and an address including street, city, state, and zip code.

### Tools:

- Power BI - Creating visualizations.
  - [Power BI Desktop Download](https://www.microsoft.com/en-us/download/details.aspx?id=58494?ocid=ORSEARCH_Bing&msockid=26de78656ac06d3a16d469376b4a6ce1)
- Microsoft SQL Server Management Studio (SSMS) - Cleaning and transforming data.
- Power Query Editor - Transforming data.

### Data Preparation

In the data preparation phase, I performed the following tasks:
1. Data transforming.
2. Data cleaning - Handling missing values.
4. Data formatting.

### Exploratory Data Analysis

EDA  involved exploring the product data to answer key questions, such as:

- Which store locations have the most order demands?
- Which product categories bring in the most revenue?
- What are the peak sales periods?

### Data Analysis

#### Transforming the data

```sql
-- converting data types for monetary values

ALTER TABLE products
ALTER COLUMN list_price DECIMAL(15,2)
ALTER TABLE order_items
ALTER COLUMN discount DECIMAL(5,2)
;

with cte as(
SELECT 
	p.product_name,
	c.category_name,
	o.order_date,
	ot.quantity,
	s.store_name,
	s.city,
	s.state,
	p.list_price,
	ot.discount
,

-- arithmetic columns

(p.list_price * (1 - discount)) as discounted_price,
((p.list_price * (1 - discount)) * quantity) as discounted_revenue,

-- formatting columns (dates)

format(order_date, 'MMM') as month,
format(order_date, 'yyyy') as year


-- combining categories table with the products table for category data

FROM products p
JOIN categories c
ON p.category_id = c.category_id

-- combining order items table with the products table for discount and quantity data

JOIN order_items ot
ON p.product_id = ot.product_id

-- combining orders table with the products table for dates

JOIN orders o
ON ot.order_id = o.order_id

-- combining stores table with the products table for city and state data

JOIN stores s
ON o.store_id = s.store_id
)

SELECT * 
FROM cte
;
```

### Data Visualization



### Key Findings

1. The Baldwin store brings in the most revenue, accounting for 67% of total revenue and far surpassing other locations, with the next highest store contributing only 20%.
2. Cruisers Bicycles outperform the second-best category by 13%, contributing 35% of total revenue across all product categories overall.
3. The revenue has shown slight fluctuations over the first two years, with the third year - 2018 - generating the highest revenue, primarily driven by a peak in sales during April. This brought in an additional R40,000 to R70,000 compared to previous years.

### Recommendations

1. Expand the Baldwin store's inventory:
   - Leverage its dominant revenue share to maximise sales opportunities and cater to higher customer demands.
2. Increase marketing focus on Cruisers Bicycles:
   - Capitalise on the category's strong performance by promoting it more aggressively.

### Limitations

- Missing total cost/unit price data:
  - The overall profitability of the bike company could not be calculated and quantified due to the absence of cost data.
 
### Next Steps

- Conduct a more granular analysis of the April 2018 revenue peak to identify specific factors contributing to the increase.

### Resources

Dataset source : [sqlservertutorial.net](https://www.sqlservertutorial.net/) , [Kaggle](https://www.kaggle.com/datasets/dillonmyrick/bike-store-sample-database?select=stores.csv)
