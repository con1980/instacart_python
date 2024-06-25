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


# Tools
I used python to load, clean, analyze and produce visualizations with the following libraries:
- pandas -> Data analysis
- numpy -> Mathematical equations
- os -> interact with operating system
- matplotlib.pyplot -> visualizations
- seaborn -> visulaizations
- scipy -> scientific equations

# Presentation
Final report with all findings are presented in an excel file.</br >
[final presentation to shareholders](</05 Sent to client>)
