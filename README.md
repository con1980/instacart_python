# Introduction
Instacart grocery basket Analysis- Python</br >
The purpose of this project is to answer business questions to improve sales and marketing of the online grocery app called INSTACART.</br >
To load, clean, analyze and produce visualizations to present my findings I used python and the applicable libraries.

# Business understanding
Instacart is an online grocery store that operates through an app. Instacart already has very good sales, but they want to uncover more information about their sales patterns.</br >
The task at hand is to perform an initial data and exploratory analysis of some of their data in order to derive insights and suggest strategies for better segmentation based on the provided criteria.</br >
To derive these insights, I need to look at the historical order data of INSTACART including their product range and customer information.</br >

### Key Questions
*	What are the busiest days of the week and hours of the day?
* Are there particular times of the day when people spend the most money?
*	Marketing and sales want to use simpler price range groupings to help direct their efforts.
*	Are there certain types of products that are more popular than others? What’s
*	What is the distribution among users in regards to their brand loyalty (How often do they return to Instacart)?
*	Are there differences in ordering habits based on a customer’s loyalty status?
*	Are there differences in ordering habits based on a customer’s region?
*	Is there a connection between age and family status in terms of ordering habits?
*	What different classifications does the demographic information suggest? Age? Income? Certain types of goods? Family status?
*	What differences can you find in ordering habits of different customer profiles?


# Data understanding
### Data sets
The data sets used in this project are real-life data sets from INSTACART, which can be downloaded from the following link:</br >
[Instacart data sets (Kaggle)](https://www.kaggle.com/datasets/psparks/instacart-market-basket-analysis)</br >
or from my github repository below</br >
[github repository](https://gist.github.com/jeremystan/c3b39d947d9b88b3ccff3147dbcf6c6b)

Note: The customer.csv data set was created for the purpose of this project.</br >
&emsp;&emsp;&emsp;Since data privacy must be protected the real-life data is of course not shared by INSTACART.

*	customers.csv -> holds all information of each customer (ie. Frist and last name, address, age, gender etc.)
*	departments.csv -> names of the different product departments in the store (ie. Bakery, beverages, frozen etc.)
*	orders_products_prior.csv -> holds the information of what kind of products each order contains.
*	orders.csv -> holds the information about all orders (ie. Order number, user id, ordered on which day of the week and hour of the day etc.)
*	products.csv -> holds all the information about the products available at INSTACART (ie. Product name, product id, aisle id, department id etc.)

Below is an overview of the orders.csv data set for illustration

![alt text](</06 Screenshots/screenshot orders data set.png>)

### Python and libraries
I am using python to load, clean, analyze and produce visualizations with the following libraries:
- pandas -> Data analysis
- numpy -> Mathematical equations
- os -> interact with operating system
- matplotlib.pyplot -> visualizations
- seaborn -> visulaizations
- scipy -> scientific equations

# Data preparation
Before diving into Analyzing and finding insights i must wrangle and clean all datasets from missing values, duplicates, mixed data types and more.
Below you can find some examples on how i cleaned the data sets at hand.

### Find and address missing values
```python
#check if there are any missing values in any columns in dataframe products
df_prods.isnull().sum()
```
```python
# extract only data with valid product names to new data frame.
df_prods_clean = df_prods[df_prods['product_name'].isnull() == False]
```
### Find and address duplicates
```python
# save duplicates in new dataframe called df_dups
df_dups = df_prods_clean[df_prods_clean.duplicated()]
# creat new df with no duplicates and no missing values
df_prods_clean_no_dups = df_prods_clean.drop_duplicates()
```
### Check for mixed data types
```python
#check for mixed types
for col in df_ords.columns.tolist():
  weird = (df_ords[[col]].map(type) != df_ords[[col]].iloc[0].apply(type)).any(axis = 1)
  if len (df_ords[weird]) > 0:
    print (col)
```
### Rename column headers to the appropriate format in the industry
```python
#rename columns of customers dataframe
df_customers=df_customers.rename(columns={'First Name': 'first_name', 'Surnam': 'last_name', 'Gender': 'gender', 'STATE': 'state', 'Age': 'age'})
```
### Derive variables
Below you can find an example where i added a loyalty flag to the dataframe to evaluate the loyalty of the customer base to answer the question of customer loyalty
```python
#Group by user_id and apply transform function on order_number with max argument to find maximum orders
#write in new column 'max_order'
ords_prods_merge['max_order'] = ords_prods_merge.groupby(['user_id'])['order_number'].transform('max')
#loc function for Loyale customer if orders over 40
ords_prods_merge.loc[ords_prods_merge['max_order'] > 40, 'loyalty_flag'] = 'Loyal customer'
#loc function for regular customer if orders between 11 and 40
ords_prods_merge.loc[(ords_prods_merge['max_order'] <= 40) & (ords_prods_merge['max_order'] > 10), 'loyalty_flag'] = 'Regular customer'
#loc function for New customer if orders 10 or lower
ords_prods_merge.loc[ords_prods_merge['max_order'] <= 10, 'loyalty_flag'] = 'New customer'
```
Below you find a count of the three loyalty groups
![alt text](</06 Screenshots/screenshot loyalty flag.png>)


### Combine dataframes
Before i can start in going into my analysis and visualization all dataframes have to be merged to another.</br >
The primary column to merge the datframes to eachother is the 'user_id' in each dataframe.</br >
```python
#check both columns are the same data type
df_customers['user_id'].dtypes == ords_prods_grouped['user_id'].dtypes
#Drop column '_merge' from last merging process
ords_prods_grouped=ords_prods_grouped.drop(['_merge'], axis=1)
#Merge the two dataframes with eachother and write it in new dataframe called 'ords_prods_customer_merged'
ords_prods_customer_merged = ords_prods_grouped.merge(df_customers, on = 'user_id', indicator = True, how ='inner')
```
Here the final Instacart merged data frame used for visualizations:

![alt text](</06 Screenshots/screenshot final data set Instacart.png>)


# Presentation
Final report with all findings are presented in an excel file.</br >
[final presentation to shareholders](</05 Sent to client>)
