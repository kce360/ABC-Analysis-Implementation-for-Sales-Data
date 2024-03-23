# ABC-Analysis-Implementation-for-Sales-Data
Simple ABC based on very simple data model

Project Description: ABC Analysis Implementation for Sales Data
ABC analysis, also known as Pareto analysis, is a method used for categorizing items based on their importance or value within a dataset. It helps in identifying the most significant contributors to overall performance, enabling better resource allocation and decision-making. In this project, ABC analysis was applied to sales data using Power Query and visualization tools to gain insights into product performance.

Two tables were created in Power Query:

**Products**
| Group      | Product  | Sales   |
|------------|----------|---------|
| Fruits     | Apple    | 630000  |
| Fruits     | Banana   | 580000  |
| Fruits     | Oranges  | 300000  |
| Vegetables | Cucumber | 120000  |
| Vegetables | Tomato   | 115000  |
| Fruits     | Plum     | 55000   |
| Fruits     | Peach    | 30000   |
| Vegetables | Beetroot | 25000   |
| Vegetables | Onion    | 20000   |
| Vegetables | Carrot   | 15000   |


**SalesTab**
| Date       | Product  | Sales   | Group      |
|------------|----------|---------|------------|
| 20.02.2021 | Oranges  | 80000   | Fruits     |
| 30.03.2021 | Oranges  | 220000  | Fruits     |
| 30.01.2021 | Banana   | 290000  | Fruits     |
| 30.03.2021 | Banana   | 290000  | Fruits     |
| 30.03.2021 | Peach    | 30000   | Fruits     |
| 30.03.2021 | Onion    | 20000   | Vegetables |
| 30.03.2021 | Carrot   | 15000   | Vegetables |
| 30.01.2021 | Cucumber | 40000   | Vegetables |
| 20.02.2021 | Cucumber | 40000   | Vegetables |
| 30.03.2021 | Cucumber | 40000   | Vegetables |
| 30.01.2021 | Tomato   | 50000   | Vegetables |
| 20.02.2021 | Tomato   | 50000   | Vegetables |
| 30.03.2021 | Tomato   | 15000   | Vegetables |
| 20.02.2021 | Apple    | 30000   | Fruits     |
| 30.03.2021 | Apple    | 300000  | Fruits     |
| 30.01.2021 | Apple    | 300000  | Fruits     |
| 30.01.2021 | Plum     | 5000    | Fruits     |
| 30.03.2021 | Plum     | 50000   | Fruits     |
| 30.03.2021 | Beetroot | 10000   | Vegetables |
| 30.01.2021 | Beetroot | 15000   | Vegetables |


The connection between the two tables was established.

**Static ABC Analysis**<br>
Sales for each product were calculated using the formula: Sales = CALCULATE(SUM(SalesTab[Sales])).

Upon realizing that some items were missing from the Sales table, a new table was created to include these missing values. This table was then appended to the main Sales table, necessitating the destruction of the automatically created relationship between Sales2 and Product tables.<br>

Cumulative Total was then calculated as follows:<br>
CumulativeTotal = <br>
 var CurrentSale = [Sales]<br>
 var BetterSale = FILTER('Products', [Sales] >= CurrentSale)<br>
 RETURN SUMX(BetterSale, [Sales])<br>

The Share was calculated using the formula: Share = DIVIDE([CumulativeTotal], SUM([Sales])).<br>

The ABC classification was defined using the formula: <br>
ABC = SWITCH(true,<br>
[Share] <= 0.8, "A",<br>
[Share] <= 0.95, "B",<br>
"C"<br>
)<br>

**The same ABC Analysis with Combined Calculation into one column**<br>
A single column, named ABCinOneColumn, was created to perform all calculations for ABC analysis. Sales amounts were added to the Products table, creating a unified dataset. Cumulative totals, shares, and ABC classifications were then calculated based on predefined thresholds.<br>

The formula for ABCinOneColumn is as follows:<br>
ABCinOneColumn = <br>
var Sale = ADDCOLUMNS('Products', "Sales2", CALCULATE(SUM(SalesTab[Sales])))<br>
var CurrentSale = CALCULATE(sum(SalesTab[Sales]))<br>
var BetterSale = FILTER(Sale, [Sales2] >= CurrentSale)<br>
var CumulativeTotal = sumx(BetterSale, [Sales2])<br>
var AllSales = CALCULATE(sum('SalesTab'[Sales]), ALL('SalesTab'))<br>
var Share = DIVIDE(CumulativeTotal, AllSales)<br>
var ABC = SWITCH(true,<br>
Share <= 0.8, "A",<br>
Share <= 0.95, "B",<br>
"C"<br>
)<br>
return ABC<br>

**Visualization**<br>
The data was visualized using a matrix to display ABC classes for each product category. Additionally, a clustered bar chart was created to showcase the share of sales and products by ABC categories. Two slicers were added to enable users to filter results by specific products and dates of sale, facilitating interactive exploration of the data.<br>
Through this project, ABC analysis was effectively applied to sales data, providing valuable insights into product performance and aiding strategic decision-making for resource allocation and inventory management.

