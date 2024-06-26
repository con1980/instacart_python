# 1. Introduction
Instacart grocery basket Analysis- Python</br >
The purpose of this project is to answer business questions to improve sales and marketing of the online grocery app called INSTACART.</br >
To load, clean, analyze and produce visualizations to present my findings I used python and the applicable libraries.

# 2. Business understanding
Instacart is an online grocery store that operates through an app. Instacart already has very good sales, but they want to uncover more information about their sales patterns.</br >
The task at hand is to perform an initial data and exploratory analysis of some of their data in order to derive insights and suggest strategies for better segmentation based on the provided criteria.</br >
To derive these insights, I need to look at the historical order data of INSTACART including their product range and customer information.</br >

### Key Questions
*	What are the busiest days of the week and hours of the day?
* Are there particular times of the day when people spend the most money?
*	Marketing and sales want to use simpler price range groupings to help direct their efforts.
*	Are there certain types of products that are more popular than others?
*	What is the distribution among users in regards to their brand loyalty (How often do they return to Instacart)?
*	Are there differences in ordering habits based on a customer’s loyalty status?
*	Are there differences in ordering habits based on a customer’s region?
*	Is there a connection between age and family status in terms of ordering habits?
*	What different classifications does the demographic information suggest? Age? Income? Certain types of goods? Family status?
*	What differences can you find in ordering habits of different customer profiles?


# 3. Data understanding

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
In this project python was used to load, clean, analyze and produce visualizations with the following libraries:
- pandas -> Data analysis
- numpy -> Mathematical equations
- os -> interact with operating system
- matplotlib.pyplot -> visualizations
- seaborn -> visulaizations
- scipy -> scientific equations

# 4. Data preparation
Before diving into Analyzing and finding insights all datasets have to be wrangled and cleaned from missing values, duplicates, mixed data types and more.
Below are some examples on how the data was cleaned.

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
### Descriptive Analysis
By looking at some descriptive analysis outliers can be identified in the dataframe and need to be fixed if necessary.</br >
Below some descriptive analysis of product prices.
```python
ords_prods_cust['prices'].describe()
```
![alt text](</06 Screenshots/screenshot prices outlier.png>)

Maximum product price is 99999 which is quite obvious a collection error and has to be dealt with.
Produce a scatterplot to see the distribution of the prices.
```python
# investigate the odd price distribution with scatterplots
sns.scatterplot(x = 'prices', y = 'prices',data = ords_prods_cust)
```
![alt text](</06 Screenshots/screenshot scatterplot prices.png>)

After evaluation, check the amount of prices which are out of range.</br >
All prices which are above 100 will be replaced by NaN values.
```python
# check for the number of out of range prices
ords_prods_cust['prices'].loc[ords_prods_cust['prices'] > 100]
# replace these prices with NaN values
ords_prods_cust.loc[ords_prods_cust['prices'] >100, 'prices'] = np.nan
```
After applying, check if the prices are now in a reasonable range.
```python
#check if executed correctly
ords_prods_cust['prices'].max()
```

### Derive variables
Below an example where a loyalty flag was added to the dataframe to evaluate the loyalty of the customer base to answer the question of customer loyalty
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
Below count of the three loyalty groups</br >
![alt text](</06 Screenshots/screenshot loyalty flag.png>)


### Combine dataframes
Before starting into the analysis and visualization all dataframes have to be merged to another.</br >
The primary column to merge the datframes in this case is the 'user_id'</br >
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

# 5. Visualizations
The best way to answer the business questions at hand is by visualize the findings.</br >
Below some of the visualizations which will clarify the questions.

### Are there particular times of the day when people order the most?
```python
#create histogram of 'order_hour_of_day'
hist_ord_hod=ords_prods_cust['order_hour_of_day'].plot.hist(bins = 24, xlabel='hour of day')
```
![alt text](</04 Analysis/Visualizations/hist_order_hour_of_day.png>)

Busiest time of the day is around 10 AM till 16 PM. After 16 PM order frequency is dropping and the lowest is from midnight till 7 AM. After that order frequency is picking up again.


### What is the distribution among users in regards to their brand loyalty?
```python
#creating bar chart from loyalty flag
bar_loyalty=ords_prods_cust['loyalty_flag'].value_counts().plot.bar(ylabel='Frequency in millions')
```
![alt text](</04 Analysis/Visualizations/bar_loyalty_new.png>)

The most are regular customers which is a very good sign. Loyal customers are on the second place. And the least are new customers.

### Are there differences in ordering habits based on a customer’s loyalty status?
```python
#create bar chart with comparison of loyalty and spending behaviour

plt.figure(figsize=(10, 6))
# define Seaborn color palette to use 
palette_color = ['#228B22','#32CD32','#90EE90']
#define column chart
bar_comp_age=sns.countplot(x = 'loyalty_flag', 
            hue = 'spending_behaviour',
            data = ords_prods_cust, 
            palette=palette_color)
bar_comp_age.set(
            ylabel='Frequency')
plt.title('Comparison Spending behaviour between loyalty type', weight='bold').set_fontsize('18')
#export visualization
plt.savefig(os.path.join(path, '04 Analysis','Visualizations', 'bar_loyalty_spending.png'))
```
![alt text](</04 Analysis/Visualizations/bar_loyalty_spending.png>)

comparing the spending behaviour of the different type of customers its obvious to see that the majority are low spending customers through all loyalty levels.

### Is there a connection between age and family status in terms of ordering habits?
```python
#create bar chart with comparison of age groups to departments. Show the top 10 departments per age group

plt.figure(figsize=(10, 6))
# define Seaborn color palette to use 
palette_color = ['#228B22','#32CD32','#90EE90']
#define column chart
bar_age_department=sns.countplot(y = 'department', 
            hue = 'age_group',
            data = complete_data, 
            palette=palette_color,
            order = complete_data['department'].value_counts().iloc[:10].index)
bar_age_department.set(
            xlabel='Counts',
            ylabel='Departments')
plt.title('Comparison Age Groups and Departments', weight='bold').set_fontsize('18')
#export visualization
plt.savefig(os.path.join(path, '04 Analysis','Visualizations', 'bar_compe_age_departments.png'))
```
![alt text](</04 Analysis/Visualizations/bar_compe_age_departments.png>)

```python
#create bar chart with comparison of dependants to departments. Show the top 10 departments per dependants group

plt.figure(figsize=(10, 6))
# define Seaborn color palette to use
palette_color = ['#228B22','#32CD32']
#define column chart
bar_dependants_department=sns.countplot(y = 'department', 
            hue = 'dependants',
            data = complete_data, 
            palette=palette_color,
            order = complete_data['department'].value_counts().iloc[:10].index)
bar_dependants_department.set(
            xlabel='Counts',
            ylabel='Departments')
plt.title('Comparison Dependants Groups with Departments', weight='bold').set_fontsize('18')
#export visualization
plt.savefig(os.path.join(path, '04 Analysis','Visualizations', 'bar_comp_dependants_departments.png'))
```
![alt text](</04 Analysis/Visualizations/bar_comp_dependants_departments.png>)

Looking at the visualizations above there is no difference in ordering habits regarding product choice between familys and age groups.
The top 5 products are produce, dairy eggs, snacks and beverages for both customer profiles.

### For a full report of all visualizations and explanations please refer to my full report:
[final presentation to shareholders](</05 Sent to client>)

# 6. Conclusions and recommendations
Through the visualizations i was able to answer all the questions from the business unit. Herewith i would like to give some recommendations on the basis of my findings.

* Busiest days of the week are Saturday and Sunday. Throughout the week its less busy. With Tuesday and Wednesday the least busy.
  When it comes to the hours, the least busy its between 21 and 7 o'clock in the morning.
  Suggestion would be to run ads on tuesday and Wednesday between 21 and 23 PM and between 6 and 7. I dont think it makes any sense to run ads in the middle of the night.
  
* People spend the most money between 8 and 14 hours. But the price difference is only 10 cents.</br >
  Question is if this little price fluctuation between the times of day would make specific advertisement economicaly feasable.
  
* The most frequent prices are in the range from 1$ to 15$ . Products in the price range from 15$ to 25$ are very rare.</br >
  It would be advisable to apply three different price groups:</br >
&emsp;- low end price group between 1$ and 5$</br >
&emsp;- mid end price group between 5 $ and 15$</br >
&emsp;- high end price group between 15 $ and 25 $</br >
  Advertisement should be heavily introduced on the low and mid end products since they have the highest frequency.

* The TOP 5 types of prducts are produce, dairy eggs, snacks, beverages and frozen.
  Instacart marketing should focus on advertising these products to keep custoemrs attention on them.
  
* The most users are regular customers. Apply a kind of reward system for regular and loyal customers so they always will come back for their purchases.
  The least users are new customers. Implement some discount insentive for new customers so they will become regular or even loyal customers in the future

### For a full review of all my recommendations please see my full report:
[final presentation to shareholders](</05 Sent to client>)






