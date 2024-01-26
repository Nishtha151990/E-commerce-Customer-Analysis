ABSTRACT
E-commerce is currently one of the fastest and dynamically evolving industries in the world. Its popularity has been growing with the ease of digital transactions and quick door-to-door deliveries. During Covid-19, I would say this has sort of become the most reliable industry without which, the existence of huge population would have been highly difficult. 
A major source of big tech companies revenues comes from the interaction of their underlying proprietary algorithms that are heavily powered by data science, so it’s fundamental to understand the methods applied to maintain and grow the number of customers, Sales, revenue etc. My project here deals with various analysis to reach out the maximum number of customers. 
RESEARCH QUESTIONS:
My E-commerce project focusses on Research on Customers as I believe client is the god of business. Right research and target could lead to huge success for any product.
So I am focusing on what Customers to focus on?
Which customers have lead to highest revenues?
What time customers bought the products leading to when should Customers be emailed contacted?


DATA DESCRIPTION
This is how my Dataset looks like. It has all the crucial information for further analysis with attributes like Customer Id, Unit Price, quantity, and Invoice Date. This data-frame contains 8 variables that correspond to:
InvoiceNo: Invoice number. Nominal, a 6-digit integral number uniquely assigned to each transaction. If this code starts with letter 'c', it indicates a cancellation.
StockCode: Product (item) code. Nominal, a 5-digit integral number uniquely assigned to each distinct product.
Description: Product (item) name. Nominal.
Quantity: The quantities of each product (item) per transaction. Numeric.
InvoiceDate: Invoice Date and time. Numeric, the day and time when each transaction was generated.
UnitPrice: Unit price. Numeric, Product price per unit in sterling.
CustomerID: Customer number. Nominal, a 5-digit integral number uniquely assigned to each customer.
Country: Country name. Nominal, the name of the country where each customer resides.
The data-frame contains 541909 entries. Below is the snapshot of what initial Data looks like


DATA CLEANING
The first step before analyzing a dataset is to preview the information it contains. To process this information easily we’re going to use Pandas, the Python library for data manipulation and analysis that offers data structures and operations for manipulating numerical tables and time series.
After proceeding with the imports of the necessary libraries, proceed with the creation of a pandas data-frame that contains the information of the dataset, and explore it.
From the result of the application of the describe() method in the “data” variable, we access to the information inside the dataset, which consists of a series of transactions made in an e-commerce platforms, for which we have identified the InvoiceNo, CustomerID and many more descriptive data that will be useful in the process.
I started exploring Data with InvoiceDate and converted str into datetime format.

In next step I explore null values and get info about column types 

While looking at the number of null values in the data-frame, it is interesting to note that ∼∼25% of the entries are not assigned to a particular customer. With the data available, it is impossible to impute values for the user and these entries are thus useless for the current exercise. So, I delete them from the data-frame:

By removing these entries, we end up with a data-frame filled at 100% for all variables duplicate entries and delete them:
I identified where does Data lie globally, Interestingly UK has huge number of records in comparison to other countries, Hence I have considered only UK data here.


It can be seen that the data concern 3950 customers and that they bought 4065 different products. The total number of transactions carried out is of the order of ∼∼23494.





Now I will determine the number of products purchased in every transaction:

The first lines of this list show several things worthy of interest:
The existence of entries with the prefix C for the InvoiceNo variable: this indicates transactions that have been cancelled. The existence of users who only came once and only purchased one product (e.g. nº12346). The existence of frequent users that buy many items at each order such as CustomerId 12747, which has occurred very frequently with high number of products, for example 14 products for invoice number 55458.
As I identified this also contains the Cancelled orders, I decided to analyze the total cancelled orders first which came out to be quite high nearly 16 % of the total transactions. Code and values are given in the picture below:

On these few lines, we see that when an order is canceled, we have other transactions in the data-frame, mostly identical except for the Quantity and InvoiceDate variables. I decide to check if this is true for all the entries. To do this, I decide to locate the entries that indicate a negative quantity and check if there is systematically an order indicating the same quantity (but positive), with the same description (CustomerID, Description and UnitPrice):

We see that the initial hypothesis is not fulfilled because of the existence of a 'Discount' entry. I check again the hypothesis but this time discarding the 'Discount' entries:



Once more, I found that the initial hypothesis is not verified. Hence, cancellations do not necessarily correspond to orders that would have been made beforehand.
At this point, I decide to create a new variable in the data-frame that indicate if part of the command has been canceled. For the cancellations without counterparts, a few of them are probably since the buy orders were performed before December 2010 (the point of entry of the database). Below, I make a census of the cancel orders and check for the existence of counterparts:


In the above function, I checked the two cases: a cancel order exists without counterpart, there is at least one counterpart with the exact same quantity
The index of the corresponding cancel order is respectively kept in the doubtfull_entry and entry_to_remove lists whose sizes are:

Among these entries, the lines listed in the doubtful_entry list correspond to the entries indicating a cancellation but for which there is no command beforehand. 


Now I check the number of entries that correspond to cancellations and that have not been deleted with the previous filter. If one looks, for example, at the purchases of the consumer of one of the above entries and corresponding to the same product as that of the cancellation, one observes:
In the next step I removed outliers, hence reducing the records.

For further analysis, I added more columns like Day, Week, Year, Month from InvoiceDate and Revenue. I calculated Revenue for every transaction, using the formula Quantity * Unit Price. Code for it is given below:













Final Dataset:
The final dataset includes 474958 rows × 16 columns. Below is the snapshot of new dataset.

VISUALIZATION:
Time for Marketing:
For any sort of marketing it’s important what time your customers made the most orders. I used my data to analyze at what month, week, hour, minute are made the most transactions. As expected in month of November and December, the Amount of transaction is highest which is also holiday and Sale season. It’s so interesting to see 48th and 49th week having highest amount of transactions which is beginning of December and end of November. (exactly close to Black Friday and before christmas, hence I can say it’s highly intutive.


For hour, Amount of transaction is nearly normally distributed. It is pretty high in the middle of the day, which is interesting. 
Best Customers:
I also analysed the Customers who bought the most, what Revenue they created and how much was the order count by them. Interestingly 15061 created highest Revenue


Interestingly count of low order size is very large which is very intuitive and count of high order size is negligible for such large data. 







Customer Acquisition:
Below analysis exhibits the number of customer acquisition month over month and in the end of dec  2011 there were more than 4k customers who made their purchase during the given period.


	

RFM ANALYSIS:
Because you cannot treat every customer the same way with the same content, same channel, same importance, you try segment customers depending on their activity. One of the most common way to do so is RFM. RFM stands for recency Frequency and Monetary Value. This will lead the segments with High, Mid and Low value.

Low Value: Customers who are less active than others, not very frequent buyer/visitor and generates very low - zero - maybe negative revenue.
Mid Value: In the middle of everything. Often using our platform (but not as much as our High Values), frequent and generates moderate revenue.
High Value: The group we do not want to lose. High Revenue, Frequency, and low Inactivity.
Recency:
Recency is calculated as the number of days from last purchase. Here, to calculate recency we used most recent purchase date of customer from InvoiceDate and the calculate for how many days customer has been inactive.
Here is the account of recency from our code. As per this data the worst 17850 customer ID last used the transaction 371 days ago which is worst amongst the provided ones here and 15291 had a transaction 25 days ago

Frequency:
Frequency is how frequent the orders were made over a period of time. Here we calculate total number of orders by customer.
Monetary/Revenue
Sales amount generated by each customer is the Revenue. We will calculate revenue for each customer. Below is table with Recency, Frequency and Revenue(monetary) value against Customer Id



3 D View of Sales vs Length of Stay vs Frequency
This chart shows that there are set of potential customers who has influential data points. This gives an intuition of finding cluster in the data.

Segmentation using KNN:
Objective of this analysis is to find pattern in the data which can group customers based on certain statistical rules. Variables which are being used are Recency, Monitory and length of stay. Below is the detailed explanation of these features.
Before applying KNN I needed to find the optimum number of clusters for which I came up with the following graph and chose the number of clusters to be 4. As after 4 the graph doesn’t show much change.



Frequency Cluster:
Frequency cluster formed using KNN, give four cluster which are in order. First cluster has large number of Records and second one has smaller nu of records. The last one has only 3 records. 



Revenue Cluster:
Here are the characteristics of Revenue cluster with 1st cluster having 3791 values with mean of 1098 and interstingly with last cluster having only 1 value hence no Standard deviation.





Overall score:



The scoring above clearly shows us that customers with score 8 is our best customers whereas 0 is the worst. The customer with 8 score has recently made a transaction which is 2 days ago, and has created large Revenue too. Whereas Customer with 0 overall score has created very little revenue with last online date of transaction nearly 306 days ago.
To keep things simple, better we name these scores:
0 to 2: Low Value
3 to 4: Mid Value
5+: High Value



CONCLUSION:
I would use this project to choose my time of marketing and segment of Customers with strategies to increase my customer base. I would not want to use all marketing strategies and target all customers with everything. Segmentation could help me identify my actions for each group. For strategies to work better I would use all the Analysis that I have used like time analysis and Customer
We can start taking actions with this segmentation. The main strategies are quite clear:
High Value: Improve Retention
Mid Value: Improve Retention + Increase Frequency
Low Value: Increase Frequency
