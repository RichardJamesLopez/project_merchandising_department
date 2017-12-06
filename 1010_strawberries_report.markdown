
# Merchandising Report using 1010data

Outlined below is a plan to increase the sale of strawberries across stores for a large grocery chain’s Merchandising Department.

## Table of Contents

1.  Introduction
1.  Three Recommendations
1.  Conclusion
1.  Appendix
  1. Background: Exploring the Dataset
  1. References

____

## Introduction
This plan below lays out three recommendations for the Merchandising department.

Each recommendation starts with an exploratory data point analysis and then walks through the rationale for each step. Assumptions for each recommendation are stated and then tested when possible. Each section concludes with actionable advice. There are even further questions that are worth exploring with each topic but are ultimately outside the scope of the recommendation.

The appendix section includes an initial assessment of the dataset, a general structure for the recommendations, and various queries that were run to test intuition.


## Recommendations
<br>Below are the 3 recommendations that I extend to the management. <br/>

1. <b>Cost Analysis</b> - Reduce the price of strawberries by $1 for the stores of #160, #189 and #82 in the state of Florida.
1. <b>Store Reorganization</b> - Reallocate inventory from the 5 stores in TN & TX to the 5 in LA & FL.
1. <b>Customer Focus Group</b> - Distribute a customer research survey and focus group for the queried list of customers in their 30s that have made 2-4 purchases of strawberries in 2014.

____
### #1 Cost Analysis

Analyzing the costs of strawberries helps determine what is driving sales for stores. Profit margins can help measure whether there is any misalignment between costs and prices, and whether certain patterns are indicative of mispricing.

In order to determine where the widest profit margins are in the country, the following query calculates "Average XSales" (extended sales) and "Average Cost". The margin is the difference between these two numbers and made into a "Profit Margin" by making it a proportion of "Average XSales". This creates a table that highlights which stores have the best and worst profit margins as a measure of healthy store business.

```
<base table="certification.retail.salesdetail_customer"/>
<link table2="certification.retail.products" col="sku" col2="sku" cols="sku,description,dept,deptdesc,group,groupdesc,pl,size,brand,manuf" shift="0"/>
<link table2="certification.retail.customer" col="customer" col2="customer" shift="0"/>
<link table2="certification.retail.stores" col="store" col2="store" shift="0"/>
<sel value="(sku=322166)"/>
<sel name="year" value="between(date;'1/1/2014';'12/31/2014')" label="2014"/>
<tabu breaks="store" label="Tabulation">
  <tcol fun="first" name="state" source="state" label="State" weight=""/>
  <tcol fun="sum" name="sum_sales" source="qty" label="Sum Sales" format="type:currency" weight=""/>
  <tcol fun="avg" name="avg_xsales" source="xsales" label="Avg_XSales" format="type:currency" weight=""/>
  <tcol fun="avg" name="avg_cost" source="cost" label="Avg_Cost" format="type:currency" weight=""/>
</tabu>
<willbe name="Margin" value="avg_xsales-avg_cost" format="type:currency"/>
<willbe name="pro_margin" label="Pro. Margin" value="Margin/avg_xsales" format="type:pct;dec:2"/>
<sort col="pro_margin" dir="down"/>
```

This is not a complete metric when sorted as such. Coupling the "Profit Margin"  with the "Average XSales", stores with the lowest "Profit Margin" also do not have the highest 2014 "Average XSales".

<br><b>Highest Profit Margins</b></br>

 ![profitmarginsbystore](/Users/RichardJamesLopez/Dropbox/RJL_-_Personal/1010/images/2017/12/profitmarginsbystore.png)


<br><b>Lowest Profit Margins</b></br>

![profitmarginsbystorelowest](/Users/RichardJamesLopez/Dropbox/RJL_-_Personal/1010/images/2017/12/profitmarginsbystorelowest.png)

This table however, is a bit taxing on the eye. To contextualize by all stores, a scatterplot will help.

![2014storesalesbyprofitmargins](/Users/RichardJamesLopez/Dropbox/RJL_-_Personal/1010/images/2017/12/2014storesalesbyprofitmargins.png)

This helps us show the that the stores with the lowest "Profit Margin" have varying sales, but not particularly low amount of "Total Sales" for 2014. However, if you combine the data from the table and this visualization, we can find a pattern among the lowest "Profit Margin" stores.


![2014storesalesbyprofitmarginszoomedin2](/Users/RichardJamesLopez/Dropbox/RJL_-_Personal/1010/images/2017/12/2014storesalesbyprofitmarginszoomedin2.png)

The stores with the thinnest margins generally come from Illinois. However, they still maintain a respectable amount of Sum of Sales. This provides data points for comparison to those stores with the widest profit margins.



![2014storesalesbyprofitmarginszoomedinbest](/Users/RichardJamesLopez/Dropbox/RJL_-_Personal/1010/images/2017/12/2014storesalesbyprofitmarginszoomedinbest.png)

Focusing on the right half of the scatterplot, Florida stores have the widest "Profit Margin". This strength, however, doesn't translate into strong "Average XSales". In fact, their sales are towards the bottom of stores that have wider profit margins.

This exploration leads me to the hypothesis that the price of strawberries in Florida is too high and can be hampering total sales.

By simply re-sorting the original table, you see that Florida has the 3 stores with the most expensive strawberries.

```
<sort col="avg_xsales" dir="down"/>
```

![highestavgxsalesbystore](/Users/RichardJamesLopez/Dropbox/RJL_-_Personal/1010/images/2017/12/highestavgxsalesbystore.png)

While this is more conclusive evidence, this is still not the complete story. This highlights that stores from Florida can be one of 2 things
1. pricing their strawberries higher than the rest (approximately $2 more than the average), or
2. selling higher quantities per transactions

The fact that Florida also has the highest profit margins supports the 1st option. If they sold higher quantities per transactions, this would still keep the profit margins constant and in line with the rest of the country.

We can test this assumption about Florida's "Average Cost" for strawberries as compared to the other states.  


![highestavgcostbystate](/Users/RichardJamesLopez/Dropbox/RJL_-_Personal/1010/images/2017/12/highestavgcostbystate.png)

Florida stores' "Average Cost" is higher than the national average by 40-50 cents. Florida stores also do not have the costliest strawberries and their price above the national average outpaces their cost above the national average. This leads me to maintain that the main driver of potential sales in Florida is more of option #1 - the prices of strawberries are too high.

<u>Recommendation</u>

Prices for strawberries in stores #160, #189 and #82 are relatively expensive and should be brought down approximately $1. This price discount would still maintain at least 50% profit margin and would test whether the change in price would increase sales.

<u>Further Questions</u>
- Delve into why there is a 15% disparity between stores with the highest and lowest "Profit Margin".
  - Is this a matter of certain distribution centers costing more and does the supply chain need to be reorganized as a result?
  - Are the cost of Strawberries for certain states indicative of prohibitive state laws and worth lobbying/relocating?

____
### # 2 Store Reorganization

Ranking store sales can help determine where are the most sales from and whether there are any patterns among their locations. By breaking down from whom and where sales are coming from, an analysis of which states are most valuable will help focus strategy for marketing, supply and relocation resources.

To begin this analysis, it makes sense to see if the customer composition by "State" is telling. An initial launch point is by looking at "Age".  

<br><u>Average Age and Transaction Count by State</u></br>
Below is the code, table and visualization for Customers by "Average Age" and their total amount of "Transactions" .

```
<base table="certification.retail.salesdetail_customer" trs_timeline_="1"/>
<link table2="certification.retail.products" col="sku" col2="sku" cols="sku,description,dept,deptdesc,group,groupdesc,pl,size,brand,manuf" trs_timeline_="1" shift="0"/>
<link table2="certification.retail.customer" col="customer" col2="customer" trs_timeline_="1" shift="0"/>
<link table2="certification.retail.stores" col="store" col2="store" trs_timeline_="1" shift="0"/>
<sel simple="1" value="(sku=322166)" trs_timeline_="1"/>
<tabu breaks="state" label="Tabulation">
  <tcol fun="avg" source="age" weight=""/>
  <tcol fun="cnt" source="transid" weight="" label="Tot. # of Transactions"/>
</tabu>
<sort col="t0" dir="up"/>

```

<br><b>Average Age of Customer by State Barchart</b></br>

![averageageofcustomerbystate](/Users/RichardJamesLopez/Dropbox/RJL_-_Personal/1010/images/2017/12/averageageofcustomerbystate.png)

<br><b>Average Age of Customer by State Table</b></br>


![averageagebybstateandtransationstable](/Users/RichardJamesLopez/Dropbox/RJL_-_Personal/1010/images/2017/12/averageagebybstateandtransationstable.png)

These demographics begin to tell a story about the "Average Age" of customer, but it makes more sense if we group this by "Region".

![averageagebyregion](/Users/RichardJamesLopez/Dropbox/RJL_-_Personal/1010/images/2017/12/averageagebyregion.png)

This doesn't get us back any meaningful disparity by "Age". However, this table indicates that close to half of sales comes from stores in the North. It is worth looking into the regional disparity and whether stores have sales disparity by region by starting off by ranking Aggregate sales by "Store".


![storeaggregatesalesalltime](/Users/RichardJamesLopez/Dropbox/RJL_-_Personal/1010/images/2017/12/storeaggregatesalesalltime.png)

This table highlights from where the highest grossing stores are coming. There doesn't appear to be a pattern to why some states have higher gross sales than others. At this point, it becomes clear that it is necessary subset the time series data when using aggregate numbers as some stores may not have been selling strawberries for the whole 4 year period. To make a comparable aggregate sales number, adding a select statement to the query will filter sales for the latest sales number.

```
<sel name="year" value="between(date;'1/1/2014';'12/31/2014')" label="2014"/>
```

This produces the following table.

![2014aggsalesperstore](/Users/RichardJamesLopez/Dropbox/RJL_-_Personal/1010/images/2017/12/2014aggsalesperstore.png)

While sorted by the highest grossing "Store", the least grossing stores are most telling.

![2014aggsalesperstorebottomrank](/Users/RichardJamesLopez/Dropbox/RJL_-_Personal/1010/images/2017/12/2014aggsalesperstorebottomrank.png)

Based on this table, there is evidence pointing to these facts regarding the higher grossing stores:
- Louisiana has only one store (#34) selling strawberries and it ranks 7th out of 34
- Florida has 4 stores and they rank 6th, 12th, 17th and 22nd out of 34 for store sales in 2014

There is also evidence pointing to these facts regarding the lower grossing stores:
- Tennessee has only one store (#94) and it ranks 31st out of 34 of stores
- Texas has 4 stores and they rank 21st, 28th, 32nd, and 34th out of 34 for store sales in 2014


To explore the merits of possible supply relocation it makes sense to look to another factor for location, the "Distribution Center".

It stands to reason that since the lower grossing stores are in a similar  geographical region as the higher grossing ones that they may share the same distribution. This doesn't seem to be the case.

<br><b>Distribution Centers for Higher Grossing States</b></br>

![2014distributionsbystateandstoretop](/Users/RichardJamesLopez/Dropbox/RJL_-_Personal/1010/images/2017/12/2014distributionsbystateandstoretop.png)

<br><b>Distribution Centers for Lower Grossing States</b></br>

![2014distributioncenterbystateandstorebottom](/Users/RichardJamesLopez/Dropbox/RJL_-_Personal/1010/images/2017/12/2014distributioncenterbystateandstorebottom.png)

While there is no clear definition on what constitutes a "Distribution Center", there appears to little overlap between the higher grossing states and the lower grossing states. Initially it seemed like a way to switch supply from the lower grossing states to the higher grossing states if they shared the same "Distribution Center".

There appears to be some insight into the performance of certain stores based on their distribution centers, but this is better analyzed with further questions in a separate analysis.


<u>Recommendation</u>

Sales figures for strawberries in stores #12, #25, #73 and #145 (all in Texas) and the store #94 (in Tennessee) lag behind other stores. Not too far within the same geographical region, the stores #10, #189, #160, #82 (all in Florida) and the store #34 (in Louisiana), these stores rank among the highest for 2014 sales. Resources should be invested into monitoring whether higher sales in Louisiana and Florida sustain or whether they run out of inventory. Should the current underperformance for the stores in Texas and in Tennessee persist and the Louisiana and Florida maintain their positive trend, supply of strawberries should be relocated from the former to the latter.

<u>Further Questions</u>
- Is there a relationship between certain Distribution Centers and store performance?
  - It appears as though the stores with the highest margins are all coming from the same distribution center, what is the underlying reason behind this relationship?
  - There seems to be that the distribution center #2 lags behind the other centers in regards to aggregate sales? Is this normal behavior or symptomatic of the service it provides to its stores?

These charts below help highlight the possible relationships.

<br><b>Distribution Center and Ranking of Sales</b></br>

![rankingsforsalesbydistributioncenter2](/Users/RichardJamesLopez/Dropbox/RJL_-_Personal/1010/images/2017/12/rankingsforsalesbydistributioncenter2.png)

<br><b>Distribution and Profit Margins</b></br>

![profitmarginbydisributioncenters](/Users/RichardJamesLopez/Dropbox/RJL_-_Personal/1010/images/2017/12/profitmarginbydisributioncenters.png)

<br><b>Distribution Centers and Aggregate Sales</b></br>

![aggregatesalesbydistributioncenter](/Users/RichardJamesLopez/Dropbox/RJL_-_Personal/1010/images/2017/12/aggregatesalesbydistributioncenter.png)

____
### #3 Customer Focus Group

Analyzing customer level data will help understand what drives the decision to purchase strawberries. Thus far, the analysis has looked at more macro variables. However, if we can identify a valuable set of customers and find out what would nudge their purchasing decisions, this can be a driver of sales. Before the step of priming customer behavior, analyzing available customer data helps explore any particular unique segments.  

A first step is to decompose the amount of transactions made by "Age" of customer.

```
<base table="certification.retail.salesdetail_customer" trs_timeline_="1"/>
<link table2="certification.retail.products" col="sku" col2="sku" cols="sku,description,dept,deptdesc,group,groupdesc,pl,size,brand,manuf" trs_timeline_="1" shift="0"/>
<link table2="certification.retail.customer" col="customer" col2="customer" trs_timeline_="1" shift="0"/>
<link table2="certification.retail.stores" col="store" col2="store" trs_timeline_="1" shift="0"/>
<sel simple="1" value="(sku=322166)" trs_timeline_="1"/>
<sort col="date" dir="down"/>
<tabu breaks="age" label="Tabulation">
  <tcol fun="cnt" source="transid" weight=""/>
</tabu>
<sort col="age" dir="up"/>
```

Here is a line chart of the query above:

<br><b>Line graph of Customer "Age" by Transaction Counts</b></br>

![transactioncountbycustomerage](/Users/RichardJamesLopez/Dropbox/RJL_-_Personal/1010/images/2017/12/transactioncountbycustomerage.png)


With this foundation, there is a visible trend for customers between ages 30-39 and a peak of transactions at the "Age" of 47. Subsetting customers by "Age" may explain if and and/or why customers increasingly buy strawberries in their 30s and then hit their peak of transactions in their 40s. Also, to remain consistent with other recommendations in this report, analysis is limited to 2014 for several reasons.

Once segmented, the variable "Format" describes the kind of store that the customers bought the strawberries from. Below is the query for 30 year olds, the adjustment to depict 40 year olds, and the two corresponding pie graphs.

```
<base table="certification.retail.salesdetail_customer" trs_timeline_="1"/>
<link table2="certification.retail.products" col="sku" col2="sku" cols="sku,description,dept,deptdesc,group,groupdesc,pl,size,brand,manuf" trs_timeline_="1" shift="0"/>
<link table2="certification.retail.customer" col="customer" col2="customer" trs_timeline_="1" shift="0"/>
<link table2="certification.retail.stores" col="store" col2="store" trs_timeline_="1" shift="0"/>
<sel simple="1" value="(sku=322166)" trs_timeline_="1"/>
<sel name="year" value="between(date;'1/1/2014';'12/31/2014')" label="2014"/>
<sel simple="1" value="between(age;'30';'39')" label="30s"/>
<tabu breaks="format" label="Tabulation" trs_timeline_="1" erroredop="tabu">
  <tcol fun="cnt" name="ct_state" source="state" label="State" weight=""/>
  <tcol name="pct_cum" fun="PGcnt" source="state" label="%"/>
</tabu>
<sort col="ct_state" dir="down"/>
```

The following line adjusts age group for the 40s is as follows:

```
<sel simple="1" value="between(age;'40';'49')" label="40s"/>
```

Visualizations for each group by the following store formats:
 - Classic
 - Grocery Fuel Super Center
 - City Tenant
 - Shopping Center

<br><b>Pie Graph for Customers 30-39 yrs old (in %)</b></br>

![storeformatcompositionfor30yearolds](/Users/RichardJamesLopez/Dropbox/RJL_-_Personal/1010/images/2017/12/storeformatcompositionfor30yearolds.png)

<br><b>Pie Graph for Customer 40-49 yrs old (in %)</b></br>

![storeformatcompositionfor40yearolds](/Users/RichardJamesLopez/Dropbox/RJL_-_Personal/1010/images/2017/12/storeformatcompositionfor40yearolds.png)

This appears to give no real indication that customers have any discernible pattern of story format based on age.

Another variable to look at is "Customer Tier".

The column for "Customer Tier" is also not terribly intuitive. To suss out its meaning, it makes sense to gather how many people are in each tier and what is the salient characteristic for the tiers.


```
<base table="certification.retail.salesdetail_customer" trs_timeline_="1"/>
<link table2="certification.retail.products" col="sku" col2="sku" cols="sku,description,dept,deptdesc,group,groupdesc,pl,size,brand,manuf" trs_timeline_="1" shift="0"/>
<link table2="certification.retail.customer" col="customer" col2="customer" trs_timeline_="1" shift="0"/>
<link table2="certification.retail.stores" col="store" col2="store" trs_timeline_="1" shift="0"/>
<sel simple="1" value="(sku=322166)" trs_timeline_="1"/>
<sort col="date" dir="down"/>
<tabu breaks="tier" label="Tabulation">
  <tcol fun="cnt" source="transid" weight=""/>
  <tcol name="pct_cum" fun="PGcnt" source="tier" label="%" format="dec:2"/>
</tabu>
<sort col="tier" dir="down"/>
```

Here is a table of the "Customer Tier" query:

![countbycustomertierinpercentallages](/Users/RichardJamesLopez/Dropbox/RJL_-_Personal/1010/images/2017/12/countbycustomertierinpercentallages.png)

There is an approximately similar proportion of customers by their tiers. Let's see if this holds between our two subsets for age.  

<br><b>Table for 30 yr Olds</b></br>

![countbycustomertierinpercent30s](/Users/RichardJamesLopez/Dropbox/RJL_-_Personal/1010/images/2017/12/countbycustomertierinpercent30s.png)

<br><b>Table for 40 yr Olds</b></br>

![countbycustomertierinpercent40s](/Users/RichardJamesLopez/Dropbox/RJL_-_Personal/1010/images/2017/12/countbycustomertierinpercent40s.png)

There appears to be a slight drop off of "Customer Tier" as customers age from their 30s to their 40s. This may be an interesting set of further questions. However, without a full definition of the goes into determining the "Customer Tier" and absent a larger shift between the demographics, this is simply evidence of a slight difference between the demographics.

The next step is querying customers by their transaction amount and aggregates sales for the year. This will help further narrow what specific kind of customer to target.

To start the query, customers in their 30s and that have bought strawberries in 2014 were analyzed.

```
<base table="certification.retail.salesdetail_customer" trs_timeline_="1"/>
<link table2="certification.retail.products" col="sku" col2="sku" cols="sku,description,dept,deptdesc,group,groupdesc,pl,size,brand,manuf" trs_timeline_="1" shift="0"/>
<link table2="certification.retail.customer" col="customer" col2="customer" trs_timeline_="1" shift="0"/>
<link table2="certification.retail.stores" col="store" col2="store" trs_timeline_="1" shift="0"/>
<sel simple="1" value="(sku=322166)" trs_timeline_="1"/>
<sel simple="1" value="between(age;'30';'39')" label="30s"/>
<sel name="year" value="between(date;'1/1/2014';'12/31/2014')" label="2014"/>
<tabu breaks="customer" label="Tabulation">
  <tcol fun="cnt" name="cnt_trans" source="transid" label="Transaction Count" weight=""/>
  <tcol fun="sum" name="sum_xsales" source="xsales" label="Sales Count" format="type:currency" weight=""/>
  <tcol fun="sum" name="sum_qty" source="qty" label="Qty Count"  weight=""/>
</tabu>
<sort col="cnt_trans" dir="down"/>
```

This narrows the number of customers to 44,458.

Narrowing the type of customers to more those who had more than 1 transaction for the year, will remove possible one-off customers that may not have a concerted interest in strawberries.

```
<sel value="(cnt_trans>1)"/>
```

By adding a select query to filter out these transactions, we get 13,506 unique customers.


![customertransactioncountaverage30s](/Users/RichardJamesLopez/Dropbox/RJL_-_Personal/1010/images/2017/12/customertransactioncountaverage30s.png)

With this sample size, we also can calculate the average amount of transactions per repeat customer. It is 3.7038.

Making one last cut of the subset of data, we can get the specific customers that closely surround this mean and filter for those who had 2-4 transactions in the year. This offers the following benefits:
- there is no need to solicit the interest of those customers who have shown they are already interested in strawberries as evidenced by repeated transactions
- gathering intel from those who only intermittently purchase strawberries can aid strategy to drive up their purchasing
- there is a reasonable amount (13,506) of customers to provide as a list for any possible marketing research.

Adding an additional select statement that limits the Customer Transaction Count to less than 5, refines the search. This brings the list of 10,681 of CUstomers in their 30s that have bought strawberries between 2 and 4 times in 2014.

This specific criterion is perfect for a customer research survey based on the following insights:
- 30 year old customers already show a growth trajectory into their 40s and it is beneficial to understand why this happens so as to further spur it
- based on a 10% assumed survey response rate (see [here](http://surveypractice.org/index.php/SurveyPractice/article/view/125/html) for research that supports this baseline), the queried list yields approximately 1000 responses for an internal team to analyze and learn from.
  - from these respondents, an extended invitation to a market focus group (assuming a further 10% participation rate, this would get us 100 focus group members) would provide even more insight.
- having this list of customers from 2014 ensures that these are recent customers that haven't been long disenfranchised with the product.



<u>Recommendation</u>

Perform a market research survey on the following list of customers:

 ![customeremailaddressbetween2and4](/Users/RichardJamesLopez/Dropbox/RJL_-_Personal/1010/images/2017/12/customeremailaddressbetween2and4.png)

This group of customers represents the most promising and growing demographics (customers in their 30s), those who have bought strawberries between 2-4 times in 2014 and have shown a minimum amount of purchasing while not being a regular customer, and represent $320,326.32 amount of current revenue.

Honing on this type of customer will frame a more focused perspective on how to market strawberries from the customers that have the most potential to drive sales growth.


Run the following query for the list:

```
<base table="certification.retail.salesdetail_customer" trs_timeline_="1"/>
<link table2="certification.retail.products" col="sku" col2="sku" cols="sku,description,dept,deptdesc,group,groupdesc,pl,size,brand,manuf" trs_timeline_="1" shift="0"/>
<link table2="certification.retail.customer" col="customer" col2="customer" trs_timeline_="1" shift="0"/>
<link table2="certification.retail.stores" col="store" col2="store" trs_timeline_="1" shift="0"/>
<sel simple="1" value="(sku=322166)" trs_timeline_="1"/>
<sel simple="1" value="between(age;'30';'39')" label="30s"/>
<sel name="year" value="between(date;'1/1/2014';'12/31/2014')" label="2014"/>
<tabu breaks="customer" label="Tabulation">
  <tcol fun="first" name="email" source="email" label="Email Address"  weight=""/>
  <tcol fun="first" name="firstname" source="firstname" label="First Name"  weight=""/>
  <tcol fun="first" name="lastname" source="lastname" label="Last Name"  weight=""/>
  <tcol fun="cnt" name="cnt_trans" source="transid" label="Transaction Count" weight=""/>
  <tcol fun="sum" name="sum_xsales" source="xsales" label="Sales Count" format="type:currency" weight=""/>
</tabu>
<sel value="(cnt_trans>1)"/>
<sel value="(cnt_trans<5)"/>
<sort col="cnt_trans" dir="down"/>
```

<u>Further Questions</u>

- Is there a relationship between "Customer Tier" and "Age"?
  - By segmenting customers into their 30s and 40s, there is a slight shift from "Customer Tier" #4&5 to #2&3. ... by further segmenting the demographics by other age ranges, would a similar relationship remain?
  - Is "Customer Tier" based on a relative metric across the whole data set, or does it have an absolute metric which sets customers apart?
    - If absolute, this will be a good indicator for experiments to see if certain activities push customers into new tiers.
- Would it be worth it to combine the 2nd and 3rd recommendations and consider surveying these demographics within the proposed states for store relocations?
  - Could we corroborate our store relocation thesis beforehand with surveys before we decide to implement a relocation?

____

## Conclusion

The three recommendations above highlight possible drivers of sales for strawberries. They uncover evidence to corroborate possible business, sales and marketing activities. With the data provided there are many actionable insights to be gleamed; these are simply highlights of the three most compelling.

While this analysis is broad, it is by no means comprehensive. Feedback is welcome and corrections are always an improvement to the product.


____

## Appendix

### <u>Exploring The Dataset</u>

This is part of the original analysis that breaks apart the sales of strawberries by state.
____
Below is the sample query and a histogram as a visualization.

```
<base table="certification.retail.salesdetail_customer" trs_timeline_="1"/>
<link table2="certification.retail.products" col="sku" col2="sku" cols="sku,description,dept,deptdesc,group,groupdesc,pl,size,brand,manuf" trs_timeline_="1" shift="0"/>
<link table2="certification.retail.customer" col="customer" col2="customer" trs_timeline_="1" shift="0"/>
<link table2="certification.retail.stores" col="store" col2="store"
trs_timeline_="1" shift="0"/>
<sel simple="1" value="(sku=322166)" trs_timeline_="1"/>
<sort col="state" dir="up"/>
<tabu breaks="state" trs_timeline_="1" label="Tabulation">
  <tcol fun="sum" source="qty" weight=""/>
</tabu>
```

<br><b>Units of Strawberries Sold by State - Full Time Range</b></br>

![unitsofstrawberriesbystate](/Users/RichardJamesLopez/Dropbox/RJL_-_Personal/1010/images/2017/12/unitsofstrawberriesbystate.png)


___
This analysis explores the time series component of the dataset.

See below for code for sorting the table based of


```
<base table="certification.retail.salesdetail_customer" trs_timeline_="1"/>
<link table2="certification.retail.products" col="sku" col2="sku" cols="sku,description,dept,deptdesc,group,groupdesc,pl,size,brand,manuf" trs_timeline_="1" shift="0"/>
<link table2="certification.retail.customer" col="customer" col2="customer" trs_timeline_="1" shift="0"/>
<link table2="certification.retail.stores" col="store" col2="store" trs_timeline_="1" shift="0"/>
<sel simple="1" value="(sku=322166)" trs_timeline_="1"/>
<sort col="date" dir="up"/>
```


To start exploring this dataset, it makes sense to gather our first datapoint of selling a Strawberries. Based on the code above that is sorted, the first date of a transaction is 10/29/2011.


Similarly, it makes sense to do the same for the last date.

```
<sort col="date" dir="down"/>
```
The last datapoint for this dataset is 12/07/2014.

____
This analysis explores the variable of "Customer Tier" in the dataset.


See below for the result:

![Customer Tier by Transanctions](/Users/RichardJamesLopez/Dropbox/RJL_-_Personal/1010/images/2017/12/customer-tier-by-transanctions.png)

___
This analysis explores the variable of Age in the dataset.


Below is a query for how many transactions by each age group. The trend line for the visualization below shows that the most amount of transactions are made between the age of 40 and 55.
```
<base table="certification.retail.salesdetail_customer" trs_timeline_="1"/>
<link table2="certification.retail.products" col="sku" col2="sku" cols="sku,description,dept,deptdesc,group,groupdesc,pl,size,brand,manuf" trs_timeline_="1" shift="0"/>
<link table2="certification.retail.customer" col="customer" col2="customer" trs_timeline_="1" shift="0"/>
<link table2="certification.retail.stores" col="store" col2="store" trs_timeline_="1" shift="0"/>
<sel simple="1" value="(sku=322166)" trs_timeline_="1"/>
<tabu breaks="age" label="Tabulation">
  <tcol fun="cnt" source="transid" weight=""/>
</tabu>
<sort col="age" dir="up" rollback_="sort"/>

```


____

This analysis explores the variable of State (for store locations) in the dataset.


 ```
 <base table="certification.retail.salesdetail_customer" trs_timeline_="1"/>
 <link table2="certification.retail.products" col="sku" col2="sku" cols="sku,description,dept,deptdesc,group,groupdesc,pl,size,brand,manuf" shift="0" trs_timeline_="1"/>
 <link table2="certification.retail.customer" col="customer" col2="customer" shift="0" trs_timeline_="1"/>
 <link table2="certification.retail.stores" col="store" col2="store" shift="0" trs_timeline_="1"/>
 <sel value="(sku=322166)" trs_timeline_="1"/>
 <tabu breaks="store" label="Tabulation" trs_timeline_="1" erroredop="tabu">
   <tcol fun="first" name="first_state" source="state" label="State of Store" weight=""/>
   <tcol fun="ucnt" name="ucnte_customers" source="customer" label="Unique Customers for Store" weight=""/>
   <tcol fun="sum" name="sum_xsales" source="xsales" label="Aggregate Extended Sales Per Store" format="type:currency" weight=""/>
   <tcol fun="first" name="sum_sqft" source="sqft" label="Square Feet of Store" weight=""/>
 </tabu>
 <willbe name="g_rank" value="g_rank(first_state;;;sum_xsales)"/>
 <sort col="first_state" dir="up"/>
 <sort col="g_rank" dir="up"/>
 ```

This leads to a hypothesis that  a certain region has stronger sales than others. To test this hypothesis, aggregate the following tables to rank the stores by their sales amongst others in the region and then in the state.  

 ```
 <base table="certification.retail.salesdetail_customer" trs_timeline_="1"/>
 <link table2="certification.retail.products" col="sku" col2="sku" cols="sku,description,dept,deptdesc,group,groupdesc,pl,size,brand,manuf" shift="0" trs_timeline_="1"/>
 <link table2="certification.retail.customer" col="customer" col2="customer" shift="0" trs_timeline_="1"/>
 <link table2="certification.retail.stores" col="store" col2="store" shift="0" trs_timeline_="1"/>
 <sel value="(sku=322166)" trs_timeline_="1"/>
 <tabu breaks="store" label="Tabulation" trs_timeline_="1" erroredop="tabu">
   <tcol fun="first" name="first_state" source="state" label="State" weight=""/>
   <tcol fun="first" name="first_region" source="divisiondesc" label="Reg. Store" />
   <tcol fun="sum" name="sum_xsales" source="xsales" label="Ag. Ex. Sales Store" format="type:currency" weight=""/>
 </tabu>
 <willbe name="g_ucnt_1" value="g_ucnt(first_state ;;store)" label="Stores in State"/>
 <willbe name="g_rank_state" value="g_rank(first_state ;;;g_ucnt_1)" label="Rk. Sales State"/>
 <willbe name="g_rank_country" value="g_rank( ;;;sum_xsales)" label="Rk. Sales in Ctry"/>
 <sort col="g_ucnt_1" dir="dn"/>
 <sort col="g_rank_state" dir="dn"/>
 <sort col="g_rank_country" dir="up"/>
 ```

<br><b>Top 10 Grossing Sales by Store - Full Time Range</b></br>

 ![Top 10 All Time](/Users/RichardJamesLopez/Dropbox/RJL_-_Personal/1010/images/2017/12/top-10-all-time.png)

<br><b>Bottom 10 Grossing Sales by Store - Full Time Range</b></br>

 ![Bottome Time All Time](/Users/RichardJamesLopez/Dropbox/RJL_-_Personal/1010/images/2017/12/bottome-time-all-time.png)

The only problem with this is that this time range is for the whole timeline of our dataset. If a store was opened halfway through this period, it would be unreasonable to expect them to rival other stores across the country that may have been selling strawberries since 2011.

To address this select rows from the tables that occur in 2014.

 ```
 <sel name="year" value="between(date;'1/1/2014';'12/31/2014')" label="2014"/>
 ```

 With this slight amendment, we can updated the snapshots and see if any one region dominates either the top or bottom ranking.


<br><b>Top Ranking Stores in Gross Sales - 2014</b></br>

 ![Top_Rankng_1994](/Users/RichardJamesLopez/Dropbox/RJL_-_Personal/1010/images/2017/12/top-rankng-1994.png)


<br><b>Bottom Ranking Stores in Gross Sales - 2014</b></br>

 ![Bottom_Ranking_2014](/Users/RichardJamesLopez/Dropbox/RJL_-_Personal/1010/images/2017/12/bottom-ranking-2014.png)


### <u>References</u>

<br><b>List of Tabulations</b></br>
https://www2.1010data.com/documentationcenter/beta/1010dataUsersGuide/index_frames.html?q=SummarizationsAndTabulations/SummarizationsAndTabulations.html
<br><b>List of Functions</b></br>
https://www2.1010data.com/documentationcenter/beta/1010dataReferenceManual/index_frames.html?q=Functions/FunctionIntro.html
<br><b>List of Group Functions</b></br>
https://www2.1010data.com/documentationcenter/beta/1010dataReferenceManual/index_frames.html?q=Functions/GroupFunctions/Group.html
<br><b>Visualizations</b></br>
https://www2.1010data.com/documentationcenter/beta/1010dataUsersGuideV10/index_frames.html?q=Glossary/macro_language.html
<br><b>List of Group Statistics</b></br>
https://www2.1010data.com/documentationcenter/beta/1010dataReferenceManual/index_frames.html?q=Functions/GroupFunctions/Group.html
<br><b>Survey Background Info</b></br>
http://surveypractice.org/index.php/SurveyPractice/article/view/125/html
