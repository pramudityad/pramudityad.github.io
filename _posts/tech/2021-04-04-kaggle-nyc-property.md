---
title: "Analysis NYC Property Sales"
categories:
  - Tech
tags:
  - analytics

  - python
---
## NYC Property Sales Introduction

The aim of this projects is to introduce you to practical statistic with Python as concrete and as consistent as possible. Using what you’ve learned; download the NYC Property Sales Dataset from Kaggle. This dataset is a record of every building or building unit (apartment, etc.) sold in the New York City property market over a 12-month period.

This dataset contains the location, address, type, sale price, and sale date of building units sold. A reference on the trickier fields:

* `BOROUGH`: A digit code for the borough the property is located in; in order these are Manhattan (1), Bronx (2), Brooklyn (3), Queens (4), and Staten Island (5).
* `BLOCK`; `LOT`: The combination of borough, block, and lot forms a unique key for property in New York City. Commonly called a BBL.
* `BUILDING CLASS AT PRESENT` and `BUILDING CLASS AT TIME OF SALE`: The type of building at various points in time.

Note that because this is a financial transaction dataset, there are some points that need to be kept in mind:

* Many sales occur with a nonsensically small dollar amount: $0 most commonly. These sales are actually transfers of deeds between parties: for example, parents transferring ownership to their home to a child after moving out for retirement.
* This dataset uses the financial definition of a building/building unit, for tax purposes. In case a single entity owns the building in question, a sale covers the value of the entire building. In case a building is owned piecemeal by its residents (a condominium), a sale refers to a single apartment (or group of apartments) owned by some individual.

Formulate a question and derive a statistical hypothesis test to answer the question. You have to demonstrate that you’re able to make decisions using data in a scientific manner. Examples of questions can be:

* Is there a difference in unit sold between property built in 1900-2000 and 2001 so on?
* Is there a difference in unit sold based on building category?
* What can you discover about New York City real estate by looking at a year's worth of raw transaction records? Can you spot trends in the market?

Please make sure that you have completed the lesson for this course, namely Python and Practical Statistics which is part of this Program.

**Note:** You can take a look at Project Rubric below:

| Code Review |  |
| :--- | :--- |
| CRITERIA | SPECIFICATIONS |
| Mean | Student implement mean to specifics column/data using pandas, numpy, or scipy|
| Median | Student implement median to specifics column/data using pandas, numpy, or scipy|
| Modus | Student implement modus to specifics column/data using pandas, numpy, or scipy|
| Central Tendencies | Implementing Central Tendencies through dataset |
| Box Plot | Implementing Box Plot to visualize spesific data |
| Z-Score | Implementing Z-score concept to specific data |
| Probability Distribution | Student analyzing distribution of data and gain insight from the distribution |
| Intervals | Implementing Confidence or Prediction Intervals |
| Hypotesis Testing | Made 1 Hypotesis and get conclusion from data |
| Preprocessing | Student preprocess dataset before applying the statistical treatment. |
| Does the code run without errors? | The code runs without errors. All code is functional and formatted properly. |

| Readability |  |
| :--- | :--- |
| CRITERIA | SPECIFICATIONS |
| Well Documented | All cell in notebook are well documented with markdown above each cell explaining the code|

| Analysis |  |
| :--- | :--- |
| CRITERIA | SPECIFICATIONS |
|Overall Analysis| Gain an insight/conclusion of overall plots that answer the hypotesis |

**Focus on "Graded-Function" sections.**

------------

## Data Preparation

Load the library you need.

Get your NYC property data from [here](https://www.kaggle.com/new-york-city/nyc-property-sales) and load the dataframe to your notebook.


```python
# Get your import statement here
import pandas as pd
import numpy as np

```


```python
# Load your dataset here
df = pd.read_csv('nyc-rolling-sales.csv')

print ('Data read into a pandas dataframe!')
```

    Data read into a pandas dataframe!


Let's view the top 5 rows of the dataset using the `head()` function.


```python
# Write your syntax here
df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Unnamed: 0</th>
      <th>BOROUGH</th>
      <th>NEIGHBORHOOD</th>
      <th>BUILDING CLASS CATEGORY</th>
      <th>TAX CLASS AT PRESENT</th>
      <th>BLOCK</th>
      <th>LOT</th>
      <th>EASE-MENT</th>
      <th>BUILDING CLASS AT PRESENT</th>
      <th>ADDRESS</th>
      <th>...</th>
      <th>RESIDENTIAL UNITS</th>
      <th>COMMERCIAL UNITS</th>
      <th>TOTAL UNITS</th>
      <th>LAND SQUARE FEET</th>
      <th>GROSS SQUARE FEET</th>
      <th>YEAR BUILT</th>
      <th>TAX CLASS AT TIME OF SALE</th>
      <th>BUILDING CLASS AT TIME OF SALE</th>
      <th>SALE PRICE</th>
      <th>SALE DATE</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>4</td>
      <td>1</td>
      <td>ALPHABET CITY</td>
      <td>07 RENTALS - WALKUP APARTMENTS</td>
      <td>2A</td>
      <td>392</td>
      <td>6</td>
      <td></td>
      <td>C2</td>
      <td>153 AVENUE B</td>
      <td>...</td>
      <td>5</td>
      <td>0</td>
      <td>5</td>
      <td>1633</td>
      <td>6440</td>
      <td>1900</td>
      <td>2</td>
      <td>C2</td>
      <td>6625000</td>
      <td>2017-07-19 00:00:00</td>
    </tr>
    <tr>
      <th>1</th>
      <td>5</td>
      <td>1</td>
      <td>ALPHABET CITY</td>
      <td>07 RENTALS - WALKUP APARTMENTS</td>
      <td>2</td>
      <td>399</td>
      <td>26</td>
      <td></td>
      <td>C7</td>
      <td>234 EAST 4TH   STREET</td>
      <td>...</td>
      <td>28</td>
      <td>3</td>
      <td>31</td>
      <td>4616</td>
      <td>18690</td>
      <td>1900</td>
      <td>2</td>
      <td>C7</td>
      <td>-</td>
      <td>2016-12-14 00:00:00</td>
    </tr>
    <tr>
      <th>2</th>
      <td>6</td>
      <td>1</td>
      <td>ALPHABET CITY</td>
      <td>07 RENTALS - WALKUP APARTMENTS</td>
      <td>2</td>
      <td>399</td>
      <td>39</td>
      <td></td>
      <td>C7</td>
      <td>197 EAST 3RD   STREET</td>
      <td>...</td>
      <td>16</td>
      <td>1</td>
      <td>17</td>
      <td>2212</td>
      <td>7803</td>
      <td>1900</td>
      <td>2</td>
      <td>C7</td>
      <td>-</td>
      <td>2016-12-09 00:00:00</td>
    </tr>
    <tr>
      <th>3</th>
      <td>7</td>
      <td>1</td>
      <td>ALPHABET CITY</td>
      <td>07 RENTALS - WALKUP APARTMENTS</td>
      <td>2B</td>
      <td>402</td>
      <td>21</td>
      <td></td>
      <td>C4</td>
      <td>154 EAST 7TH STREET</td>
      <td>...</td>
      <td>10</td>
      <td>0</td>
      <td>10</td>
      <td>2272</td>
      <td>6794</td>
      <td>1913</td>
      <td>2</td>
      <td>C4</td>
      <td>3936272</td>
      <td>2016-09-23 00:00:00</td>
    </tr>
    <tr>
      <th>4</th>
      <td>8</td>
      <td>1</td>
      <td>ALPHABET CITY</td>
      <td>07 RENTALS - WALKUP APARTMENTS</td>
      <td>2A</td>
      <td>404</td>
      <td>55</td>
      <td></td>
      <td>C2</td>
      <td>301 EAST 10TH   STREET</td>
      <td>...</td>
      <td>6</td>
      <td>0</td>
      <td>6</td>
      <td>2369</td>
      <td>4615</td>
      <td>1900</td>
      <td>2</td>
      <td>C2</td>
      <td>8000000</td>
      <td>2016-11-17 00:00:00</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 22 columns</p>
</div>



We can also veiw the bottom 5 rows of the dataset using the `tail()` function.


```python
# Write your syntax here
df.tail()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Unnamed: 0</th>
      <th>BOROUGH</th>
      <th>NEIGHBORHOOD</th>
      <th>BUILDING CLASS CATEGORY</th>
      <th>TAX CLASS AT PRESENT</th>
      <th>BLOCK</th>
      <th>LOT</th>
      <th>EASE-MENT</th>
      <th>BUILDING CLASS AT PRESENT</th>
      <th>ADDRESS</th>
      <th>...</th>
      <th>RESIDENTIAL UNITS</th>
      <th>COMMERCIAL UNITS</th>
      <th>TOTAL UNITS</th>
      <th>LAND SQUARE FEET</th>
      <th>GROSS SQUARE FEET</th>
      <th>YEAR BUILT</th>
      <th>TAX CLASS AT TIME OF SALE</th>
      <th>BUILDING CLASS AT TIME OF SALE</th>
      <th>SALE PRICE</th>
      <th>SALE DATE</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>84543</th>
      <td>8409</td>
      <td>5</td>
      <td>WOODROW</td>
      <td>02 TWO FAMILY DWELLINGS</td>
      <td>1</td>
      <td>7349</td>
      <td>34</td>
      <td></td>
      <td>B9</td>
      <td>37 QUAIL LANE</td>
      <td>...</td>
      <td>2</td>
      <td>0</td>
      <td>2</td>
      <td>2400</td>
      <td>2575</td>
      <td>1998</td>
      <td>1</td>
      <td>B9</td>
      <td>450000</td>
      <td>2016-11-28 00:00:00</td>
    </tr>
    <tr>
      <th>84544</th>
      <td>8410</td>
      <td>5</td>
      <td>WOODROW</td>
      <td>02 TWO FAMILY DWELLINGS</td>
      <td>1</td>
      <td>7349</td>
      <td>78</td>
      <td></td>
      <td>B9</td>
      <td>32 PHEASANT LANE</td>
      <td>...</td>
      <td>2</td>
      <td>0</td>
      <td>2</td>
      <td>2498</td>
      <td>2377</td>
      <td>1998</td>
      <td>1</td>
      <td>B9</td>
      <td>550000</td>
      <td>2017-04-21 00:00:00</td>
    </tr>
    <tr>
      <th>84545</th>
      <td>8411</td>
      <td>5</td>
      <td>WOODROW</td>
      <td>02 TWO FAMILY DWELLINGS</td>
      <td>1</td>
      <td>7351</td>
      <td>60</td>
      <td></td>
      <td>B2</td>
      <td>49 PITNEY AVENUE</td>
      <td>...</td>
      <td>2</td>
      <td>0</td>
      <td>2</td>
      <td>4000</td>
      <td>1496</td>
      <td>1925</td>
      <td>1</td>
      <td>B2</td>
      <td>460000</td>
      <td>2017-07-05 00:00:00</td>
    </tr>
    <tr>
      <th>84546</th>
      <td>8412</td>
      <td>5</td>
      <td>WOODROW</td>
      <td>22 STORE BUILDINGS</td>
      <td>4</td>
      <td>7100</td>
      <td>28</td>
      <td></td>
      <td>K6</td>
      <td>2730 ARTHUR KILL ROAD</td>
      <td>...</td>
      <td>0</td>
      <td>7</td>
      <td>7</td>
      <td>208033</td>
      <td>64117</td>
      <td>2001</td>
      <td>4</td>
      <td>K6</td>
      <td>11693337</td>
      <td>2016-12-21 00:00:00</td>
    </tr>
    <tr>
      <th>84547</th>
      <td>8413</td>
      <td>5</td>
      <td>WOODROW</td>
      <td>35 INDOOR PUBLIC AND CULTURAL FACILITIES</td>
      <td>4</td>
      <td>7105</td>
      <td>679</td>
      <td></td>
      <td>P9</td>
      <td>155 CLAY PIT ROAD</td>
      <td>...</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>10796</td>
      <td>2400</td>
      <td>2006</td>
      <td>4</td>
      <td>P9</td>
      <td>69300</td>
      <td>2016-10-27 00:00:00</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 22 columns</p>
</div>



BOROUGH: A digit code for the borough the property is located in; in order these are Manhattan (1), Bronx (2), Brooklyn (3), Queens (4), and Staten Island (5).

To view the dimensions of the dataframe, we use the `.shape` parameter. Expected result: (84548, 22)


```python
# Write your syntax here
df.shape
```




    (84548, 22)



According to this official page, Ease-ment is "is a right, such as a right of way, which allows an entity to make limited use of another’s real property. For example: MTA railroad tracks that run across a portion of another property". Also, the Unnamed column is not mentioned and was likely used for iterating through records. So, those two columns are removed for now.


```python
# Drop 'Unnamed: 0' and 'EASE-MENT' features using .drop function
df.drop(['Unnamed: 0','EASE-MENT'], axis=1, inplace=True)
df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>BOROUGH</th>
      <th>NEIGHBORHOOD</th>
      <th>BUILDING CLASS CATEGORY</th>
      <th>TAX CLASS AT PRESENT</th>
      <th>BLOCK</th>
      <th>LOT</th>
      <th>BUILDING CLASS AT PRESENT</th>
      <th>ADDRESS</th>
      <th>APARTMENT NUMBER</th>
      <th>ZIP CODE</th>
      <th>RESIDENTIAL UNITS</th>
      <th>COMMERCIAL UNITS</th>
      <th>TOTAL UNITS</th>
      <th>LAND SQUARE FEET</th>
      <th>GROSS SQUARE FEET</th>
      <th>YEAR BUILT</th>
      <th>TAX CLASS AT TIME OF SALE</th>
      <th>BUILDING CLASS AT TIME OF SALE</th>
      <th>SALE PRICE</th>
      <th>SALE DATE</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>ALPHABET CITY</td>
      <td>07 RENTALS - WALKUP APARTMENTS</td>
      <td>2A</td>
      <td>392</td>
      <td>6</td>
      <td>C2</td>
      <td>153 AVENUE B</td>
      <td></td>
      <td>10009</td>
      <td>5</td>
      <td>0</td>
      <td>5</td>
      <td>1633</td>
      <td>6440</td>
      <td>1900</td>
      <td>2</td>
      <td>C2</td>
      <td>6625000</td>
      <td>2017-07-19 00:00:00</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>ALPHABET CITY</td>
      <td>07 RENTALS - WALKUP APARTMENTS</td>
      <td>2</td>
      <td>399</td>
      <td>26</td>
      <td>C7</td>
      <td>234 EAST 4TH   STREET</td>
      <td></td>
      <td>10009</td>
      <td>28</td>
      <td>3</td>
      <td>31</td>
      <td>4616</td>
      <td>18690</td>
      <td>1900</td>
      <td>2</td>
      <td>C7</td>
      <td>-</td>
      <td>2016-12-14 00:00:00</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1</td>
      <td>ALPHABET CITY</td>
      <td>07 RENTALS - WALKUP APARTMENTS</td>
      <td>2</td>
      <td>399</td>
      <td>39</td>
      <td>C7</td>
      <td>197 EAST 3RD   STREET</td>
      <td></td>
      <td>10009</td>
      <td>16</td>
      <td>1</td>
      <td>17</td>
      <td>2212</td>
      <td>7803</td>
      <td>1900</td>
      <td>2</td>
      <td>C7</td>
      <td>-</td>
      <td>2016-12-09 00:00:00</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1</td>
      <td>ALPHABET CITY</td>
      <td>07 RENTALS - WALKUP APARTMENTS</td>
      <td>2B</td>
      <td>402</td>
      <td>21</td>
      <td>C4</td>
      <td>154 EAST 7TH STREET</td>
      <td></td>
      <td>10009</td>
      <td>10</td>
      <td>0</td>
      <td>10</td>
      <td>2272</td>
      <td>6794</td>
      <td>1913</td>
      <td>2</td>
      <td>C4</td>
      <td>3936272</td>
      <td>2016-09-23 00:00:00</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1</td>
      <td>ALPHABET CITY</td>
      <td>07 RENTALS - WALKUP APARTMENTS</td>
      <td>2A</td>
      <td>404</td>
      <td>55</td>
      <td>C2</td>
      <td>301 EAST 10TH   STREET</td>
      <td></td>
      <td>10009</td>
      <td>6</td>
      <td>0</td>
      <td>6</td>
      <td>2369</td>
      <td>4615</td>
      <td>1900</td>
      <td>2</td>
      <td>C2</td>
      <td>8000000</td>
      <td>2016-11-17 00:00:00</td>
    </tr>
  </tbody>
</table>
</div>



Let's view Dtype of each features in dataframe using `.info()` function.


```python
df.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 84548 entries, 0 to 84547
    Data columns (total 20 columns):
     #   Column                          Non-Null Count  Dtype 
    ---  ------                          --------------  ----- 
     0   BOROUGH                         84548 non-null  int64 
     1   NEIGHBORHOOD                    84548 non-null  object
     2   BUILDING CLASS CATEGORY         84548 non-null  object
     3   TAX CLASS AT PRESENT            84548 non-null  object
     4   BLOCK                           84548 non-null  int64 
     5   LOT                             84548 non-null  int64 
     6   BUILDING CLASS AT PRESENT       84548 non-null  object
     7   ADDRESS                         84548 non-null  object
     8   APARTMENT NUMBER                84548 non-null  object
     9   ZIP CODE                        84548 non-null  int64 
     10  RESIDENTIAL UNITS               84548 non-null  int64 
     11  COMMERCIAL UNITS                84548 non-null  int64 
     12  TOTAL UNITS                     84548 non-null  int64 
     13  LAND SQUARE FEET                84548 non-null  object
     14  GROSS SQUARE FEET               84548 non-null  object
     15  YEAR BUILT                      84548 non-null  int64 
     16  TAX CLASS AT TIME OF SALE       84548 non-null  int64 
     17  BUILDING CLASS AT TIME OF SALE  84548 non-null  object
     18  SALE PRICE                      84548 non-null  object
     19  SALE DATE                       84548 non-null  object
    dtypes: int64(9), object(11)
    memory usage: 12.9+ MB


It looks like empty records are not being treated as NA. We convert columns to their appropriate data types to obtain NAs.


```python
#First, let's check which columns should be categorical
print('Column name')
for col in df.columns:
    if df[col].dtype=='object':
        print(col, df[col].nunique())
```

    Column name
    NEIGHBORHOOD 254
    BUILDING CLASS CATEGORY 47
    TAX CLASS AT PRESENT 11
    BUILDING CLASS AT PRESENT 167
    ADDRESS 67563
    APARTMENT NUMBER 3989
    LAND SQUARE FEET 6062
    GROSS SQUARE FEET 5691
    BUILDING CLASS AT TIME OF SALE 166
    SALE PRICE 10008
    SALE DATE 364



```python
# LAND SQUARE FEET,GROSS SQUARE FEET, SALE PRICE, BOROUGH should be numeric. 
# SALE DATE datetime format.
# categorical: NEIGHBORHOOD, BUILDING CLASS CATEGORY, TAX CLASS AT PRESENT, BUILDING CLASS AT PRESENT,
# BUILDING CLASS AT TIME OF SALE, TAX CLASS AT TIME OF SALE,BOROUGH 

numer = ['LAND SQUARE FEET','GROSS SQUARE FEET', 'SALE PRICE', 'BOROUGH']
for col in numer: # coerce for missing values
    df[col] = pd.to_numeric(df[col], errors='coerce')

categ = ['NEIGHBORHOOD', 'BUILDING CLASS CATEGORY', 'TAX CLASS AT PRESENT', 'BUILDING CLASS AT PRESENT', 'BUILDING CLASS AT TIME OF SALE', 'TAX CLASS AT TIME OF SALE']
for col in categ:
    df[col] = df[col].astype('category')

df['SALE DATE'] = pd.to_datetime(df['SALE DATE'], errors='coerce')
```

Our dataset is ready for checking missing values.


```python
missing = df.isnull().sum()/len(df)*100

print(pd.DataFrame([missing[missing>0],pd.Series(df.isnull().sum()[df.isnull().sum()>1000])], index=['percent missing','how many missing']))
```

                      LAND SQUARE FEET  GROSS SQUARE FEET   SALE PRICE
    percent missing          31.049818          32.658372     17.22217
    how many missing      26252.000000       27612.000000  14561.00000


Around 30% of GROSS SF and LAND SF are missing. Furthermore, around 17% of SALE PRICE is also missing.

We can fill in the missing value from one column to another, which will help us reduce missing values. Expected values:

(6, 20)

(1366, 20)


```python
print(df[(df['LAND SQUARE FEET'].isnull()) & (df['GROSS SQUARE FEET'].notnull())].shape)
print(df[(df['LAND SQUARE FEET'].notnull()) & (df['GROSS SQUARE FEET'].isnull())].shape)
```

    (6, 20)
    (1366, 20)


There are 1372 rows that can be filled in with their approximate values.


```python
df['LAND SQUARE FEET'] = df['LAND SQUARE FEET'].mask((df['LAND SQUARE FEET'].isnull()) & (df['GROSS SQUARE FEET'].notnull()), df['GROSS SQUARE FEET'])
df['GROSS SQUARE FEET'] = df['GROSS SQUARE FEET'].mask((df['LAND SQUARE FEET'].notnull()) & (df['GROSS SQUARE FEET'].isnull()), df['LAND SQUARE FEET'])
```


```python
#  Check for duplicates before

print(sum(df.duplicated()))

df[df.duplicated(keep=False)].sort_values(['NEIGHBORHOOD', 'ADDRESS']).head(10)

# df.duplicated() automatically excludes duplicates, to keep duplicates in df we use keep=False

# in df.duplicated(df.columns) we can specify column names to look for duplicates only in those mentioned columns.
```

    765





<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>BOROUGH</th>
      <th>NEIGHBORHOOD</th>
      <th>BUILDING CLASS CATEGORY</th>
      <th>TAX CLASS AT PRESENT</th>
      <th>BLOCK</th>
      <th>LOT</th>
      <th>BUILDING CLASS AT PRESENT</th>
      <th>ADDRESS</th>
      <th>APARTMENT NUMBER</th>
      <th>ZIP CODE</th>
      <th>RESIDENTIAL UNITS</th>
      <th>COMMERCIAL UNITS</th>
      <th>TOTAL UNITS</th>
      <th>LAND SQUARE FEET</th>
      <th>GROSS SQUARE FEET</th>
      <th>YEAR BUILT</th>
      <th>TAX CLASS AT TIME OF SALE</th>
      <th>BUILDING CLASS AT TIME OF SALE</th>
      <th>SALE PRICE</th>
      <th>SALE DATE</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>76286</th>
      <td>5</td>
      <td>ANNADALE</td>
      <td>02 TWO FAMILY DWELLINGS</td>
      <td>1</td>
      <td>6350</td>
      <td>7</td>
      <td>B2</td>
      <td>106 BENNETT PLACE</td>
      <td></td>
      <td>10312</td>
      <td>2</td>
      <td>0</td>
      <td>2</td>
      <td>8000.0</td>
      <td>4208.0</td>
      <td>1985</td>
      <td>1</td>
      <td>B2</td>
      <td>NaN</td>
      <td>2017-06-27</td>
    </tr>
    <tr>
      <th>76287</th>
      <td>5</td>
      <td>ANNADALE</td>
      <td>02 TWO FAMILY DWELLINGS</td>
      <td>1</td>
      <td>6350</td>
      <td>7</td>
      <td>B2</td>
      <td>106 BENNETT PLACE</td>
      <td></td>
      <td>10312</td>
      <td>2</td>
      <td>0</td>
      <td>2</td>
      <td>8000.0</td>
      <td>4208.0</td>
      <td>1985</td>
      <td>1</td>
      <td>B2</td>
      <td>NaN</td>
      <td>2017-06-27</td>
    </tr>
    <tr>
      <th>76322</th>
      <td>5</td>
      <td>ANNADALE</td>
      <td>05 TAX CLASS 1 VACANT LAND</td>
      <td>1B</td>
      <td>6459</td>
      <td>28</td>
      <td>V0</td>
      <td>N/A HYLAN BOULEVARD</td>
      <td></td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>6667.0</td>
      <td>6667.0</td>
      <td>0</td>
      <td>1</td>
      <td>V0</td>
      <td>NaN</td>
      <td>2017-05-11</td>
    </tr>
    <tr>
      <th>76323</th>
      <td>5</td>
      <td>ANNADALE</td>
      <td>05 TAX CLASS 1 VACANT LAND</td>
      <td>1B</td>
      <td>6459</td>
      <td>28</td>
      <td>V0</td>
      <td>N/A HYLAN BOULEVARD</td>
      <td></td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>6667.0</td>
      <td>6667.0</td>
      <td>0</td>
      <td>1</td>
      <td>V0</td>
      <td>NaN</td>
      <td>2017-05-11</td>
    </tr>
    <tr>
      <th>76383</th>
      <td>5</td>
      <td>ARDEN HEIGHTS</td>
      <td>01 ONE FAMILY DWELLINGS</td>
      <td>1</td>
      <td>5741</td>
      <td>93</td>
      <td>A5</td>
      <td>266 ILYSSA WAY</td>
      <td></td>
      <td>10312</td>
      <td>1</td>
      <td>0</td>
      <td>1</td>
      <td>500.0</td>
      <td>1354.0</td>
      <td>1996</td>
      <td>1</td>
      <td>A5</td>
      <td>320000.0</td>
      <td>2017-06-06</td>
    </tr>
    <tr>
      <th>76384</th>
      <td>5</td>
      <td>ARDEN HEIGHTS</td>
      <td>01 ONE FAMILY DWELLINGS</td>
      <td>1</td>
      <td>5741</td>
      <td>93</td>
      <td>A5</td>
      <td>266 ILYSSA WAY</td>
      <td></td>
      <td>10312</td>
      <td>1</td>
      <td>0</td>
      <td>1</td>
      <td>500.0</td>
      <td>1354.0</td>
      <td>1996</td>
      <td>1</td>
      <td>A5</td>
      <td>320000.0</td>
      <td>2017-06-06</td>
    </tr>
    <tr>
      <th>76643</th>
      <td>5</td>
      <td>ARROCHAR</td>
      <td>02 TWO FAMILY DWELLINGS</td>
      <td>1</td>
      <td>3103</td>
      <td>57</td>
      <td>B2</td>
      <td>129 MC CLEAN AVENUE</td>
      <td></td>
      <td>10305</td>
      <td>2</td>
      <td>0</td>
      <td>2</td>
      <td>5000.0</td>
      <td>2733.0</td>
      <td>1925</td>
      <td>1</td>
      <td>B2</td>
      <td>NaN</td>
      <td>2017-03-21</td>
    </tr>
    <tr>
      <th>76644</th>
      <td>5</td>
      <td>ARROCHAR</td>
      <td>02 TWO FAMILY DWELLINGS</td>
      <td>1</td>
      <td>3103</td>
      <td>57</td>
      <td>B2</td>
      <td>129 MC CLEAN AVENUE</td>
      <td></td>
      <td>10305</td>
      <td>2</td>
      <td>0</td>
      <td>2</td>
      <td>5000.0</td>
      <td>2733.0</td>
      <td>1925</td>
      <td>1</td>
      <td>B2</td>
      <td>NaN</td>
      <td>2017-03-21</td>
    </tr>
    <tr>
      <th>50126</th>
      <td>4</td>
      <td>ASTORIA</td>
      <td>03 THREE FAMILY DWELLINGS</td>
      <td>1</td>
      <td>856</td>
      <td>139</td>
      <td>C0</td>
      <td>22-18 27TH   STREET</td>
      <td></td>
      <td>11105</td>
      <td>3</td>
      <td>0</td>
      <td>3</td>
      <td>2000.0</td>
      <td>1400.0</td>
      <td>1930</td>
      <td>1</td>
      <td>C0</td>
      <td>NaN</td>
      <td>2017-01-12</td>
    </tr>
    <tr>
      <th>50127</th>
      <td>4</td>
      <td>ASTORIA</td>
      <td>03 THREE FAMILY DWELLINGS</td>
      <td>1</td>
      <td>856</td>
      <td>139</td>
      <td>C0</td>
      <td>22-18 27TH   STREET</td>
      <td></td>
      <td>11105</td>
      <td>3</td>
      <td>0</td>
      <td>3</td>
      <td>2000.0</td>
      <td>1400.0</td>
      <td>1930</td>
      <td>1</td>
      <td>C0</td>
      <td>NaN</td>
      <td>2017-01-12</td>
    </tr>
  </tbody>
</table>
</div>



The dataframe has 765 duplicated rows (exluding the original rows).


```python
df.drop_duplicates(inplace=True)

print(sum(df.duplicated()))
```

    0


## Exploratory data analysis

Now, let's get a simple descriptive statistics with `.describe()` function for `COMMERCIAL UNITS` features.


```python
df[df['COMMERCIAL UNITS']==0].describe().transpose()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>count</th>
      <th>mean</th>
      <th>std</th>
      <th>min</th>
      <th>25%</th>
      <th>50%</th>
      <th>75%</th>
      <th>max</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>BOROUGH</th>
      <td>78777.0</td>
      <td>3.004329</td>
      <td>1.298594e+00</td>
      <td>1.0</td>
      <td>2.0</td>
      <td>3.0</td>
      <td>4.0</td>
      <td>5.0</td>
    </tr>
    <tr>
      <th>BLOCK</th>
      <td>78777.0</td>
      <td>4273.781015</td>
      <td>3.589242e+03</td>
      <td>1.0</td>
      <td>1330.0</td>
      <td>3340.0</td>
      <td>6361.0</td>
      <td>16322.0</td>
    </tr>
    <tr>
      <th>LOT</th>
      <td>78777.0</td>
      <td>395.422420</td>
      <td>6.716047e+02</td>
      <td>1.0</td>
      <td>23.0</td>
      <td>52.0</td>
      <td>1003.0</td>
      <td>9106.0</td>
    </tr>
    <tr>
      <th>ZIP CODE</th>
      <td>78777.0</td>
      <td>10722.737068</td>
      <td>1.318494e+03</td>
      <td>0.0</td>
      <td>10304.0</td>
      <td>11209.0</td>
      <td>11357.0</td>
      <td>11694.0</td>
    </tr>
    <tr>
      <th>RESIDENTIAL UNITS</th>
      <td>78777.0</td>
      <td>1.691737</td>
      <td>9.838994e+00</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>2.0</td>
      <td>889.0</td>
    </tr>
    <tr>
      <th>COMMERCIAL UNITS</th>
      <td>78777.0</td>
      <td>0.000000</td>
      <td>0.000000e+00</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>TOTAL UNITS</th>
      <td>78777.0</td>
      <td>1.724133</td>
      <td>9.835016e+00</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>2.0</td>
      <td>889.0</td>
    </tr>
    <tr>
      <th>LAND SQUARE FEET</th>
      <td>52780.0</td>
      <td>3140.139731</td>
      <td>2.929999e+04</td>
      <td>0.0</td>
      <td>1600.0</td>
      <td>2295.0</td>
      <td>3300.0</td>
      <td>4252327.0</td>
    </tr>
    <tr>
      <th>GROSS SQUARE FEET</th>
      <td>52780.0</td>
      <td>2714.612069</td>
      <td>2.791294e+04</td>
      <td>0.0</td>
      <td>975.0</td>
      <td>1600.0</td>
      <td>2388.0</td>
      <td>4252327.0</td>
    </tr>
    <tr>
      <th>YEAR BUILT</th>
      <td>78777.0</td>
      <td>1781.065451</td>
      <td>5.510246e+02</td>
      <td>0.0</td>
      <td>1920.0</td>
      <td>1940.0</td>
      <td>1967.0</td>
      <td>2017.0</td>
    </tr>
    <tr>
      <th>SALE PRICE</th>
      <td>65629.0</td>
      <td>995296.912904</td>
      <td>3.329268e+06</td>
      <td>0.0</td>
      <td>240000.0</td>
      <td>529490.0</td>
      <td>921956.0</td>
      <td>345000000.0</td>
    </tr>
  </tbody>
</table>
</div>



Let us try to understand the columns. Above table shows descriptive statistics for the numeric columns.

- There are zipcodes with 0 value
- Can block/lot numbers go up to 16322?
- Most of the properties have 2 unit and maximum of 1844 units? The latter might mean some company purchased a building. This should be treated as an outlier.
- Other columns also have outliers which needs further investigation.
- Year column has a year with 0
- Most sales prices less than 10000 can be treated as gift or transfer fees.

Now, let's get a simple descriptive statistics with `.describe()` function for `RESIDENTIAL UNITS` features.




```python
# Write your function below
df[df['RESIDENTIAL UNITS']==0].describe().transpose()
# Graded-Funtion Begin (~1 Lines)

# Graded-Funtion End
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>count</th>
      <th>mean</th>
      <th>std</th>
      <th>min</th>
      <th>25%</th>
      <th>50%</th>
      <th>75%</th>
      <th>max</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>BOROUGH</th>
      <td>24546.0</td>
      <td>2.542084e+00</td>
      <td>1.334486e+00</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>3.0</td>
      <td>4.00</td>
      <td>5.000000e+00</td>
    </tr>
    <tr>
      <th>BLOCK</th>
      <td>24546.0</td>
      <td>3.355267e+03</td>
      <td>3.091222e+03</td>
      <td>1.0</td>
      <td>1158.0</td>
      <td>1947.0</td>
      <td>5390.75</td>
      <td>1.631700e+04</td>
    </tr>
    <tr>
      <th>LOT</th>
      <td>24546.0</td>
      <td>2.839434e+02</td>
      <td>5.700453e+02</td>
      <td>1.0</td>
      <td>12.0</td>
      <td>38.0</td>
      <td>135.00</td>
      <td>9.056000e+03</td>
    </tr>
    <tr>
      <th>ZIP CODE</th>
      <td>24546.0</td>
      <td>1.032151e+04</td>
      <td>2.135406e+03</td>
      <td>0.0</td>
      <td>10023.0</td>
      <td>11004.0</td>
      <td>11354.00</td>
      <td>1.169400e+04</td>
    </tr>
    <tr>
      <th>RESIDENTIAL UNITS</th>
      <td>24546.0</td>
      <td>0.000000e+00</td>
      <td>0.000000e+00</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.00</td>
      <td>0.000000e+00</td>
    </tr>
    <tr>
      <th>COMMERCIAL UNITS</th>
      <td>24546.0</td>
      <td>4.593824e-01</td>
      <td>1.582602e+01</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.00</td>
      <td>2.261000e+03</td>
    </tr>
    <tr>
      <th>TOTAL UNITS</th>
      <td>24546.0</td>
      <td>5.633504e-01</td>
      <td>1.582595e+01</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.00</td>
      <td>2.261000e+03</td>
    </tr>
    <tr>
      <th>LAND SQUARE FEET</th>
      <td>9503.0</td>
      <td>7.416797e+03</td>
      <td>8.032892e+04</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>3250.00</td>
      <td>4.252327e+06</td>
    </tr>
    <tr>
      <th>GROSS SQUARE FEET</th>
      <td>9503.0</td>
      <td>8.870466e+03</td>
      <td>7.890877e+04</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>2500.00</td>
      <td>4.252327e+06</td>
    </tr>
    <tr>
      <th>YEAR BUILT</th>
      <td>24546.0</td>
      <td>1.675526e+03</td>
      <td>6.790950e+02</td>
      <td>0.0</td>
      <td>1921.0</td>
      <td>1950.0</td>
      <td>1962.00</td>
      <td>2.017000e+03</td>
    </tr>
    <tr>
      <th>SALE PRICE</th>
      <td>20855.0</td>
      <td>1.632257e+06</td>
      <td>1.969307e+07</td>
      <td>0.0</td>
      <td>182500.0</td>
      <td>395000.0</td>
      <td>850000.00</td>
      <td>2.210000e+09</td>
    </tr>
  </tbody>
</table>
</div>



Write your findings below:



Use `.value_counts` function to count total value of `BOROUGH` features. Expected value:

4    26548\
3    23843\
1    18102\
5     8296\
2     6994\
Name: BOROUGH, dtype: int64


```python
print('uniqe value ',df['BOROUGH'].unique())
df1 = df['BOROUGH'].groupby(df['BOROUGH']).value_counts()
df1
```

    uniqe value  [1 2 3 4 5]





    BOROUGH  BOROUGH
    1        1          18102
    2        2           6994
    3        3          23843
    4        4          26548
    5        5           8296
    Name: BOROUGH, dtype: int64



From here, we can calculate the mean for each Borough. Use `.mean()` function to calculate mean.




```python
# Write your function below
df3 = df['SALE PRICE'].groupby(df['BOROUGH']).value_counts()
# Graded-Funtion Begin (~1 Lines)
print(df3)
# Graded-Funtion End
print('mean ',df3.mean())
```

    BOROUGH  SALE PRICE 
    1        10.0           90
             1100000.0      89
             750000.0       82
             1300000.0      79
             1250000.0      77
                            ..
    5        11700000.0      1
             11900000.0      1
             31500000.0      1
             67200000.0      1
             122000000.0     1
    Name: SALE PRICE, Length: 14997, dtype: int64
    mean  4.641394945655798


From here, we can calculate the median for each Borough. Use `.median()` function to calculate median.




```python
# Write your function below
print(df3.describe())
print('median ',df3.median())
# Graded-Funtion Begin (~1 Lines)

# Graded-Funtion End
```

    count    14997.000000
    mean         4.641395
    std         69.275739
    min          1.000000
    25%          1.000000
    50%          1.000000
    75%          2.000000
    max       8186.000000
    Name: SALE PRICE, dtype: float64
    median  1.0


From here, we can calculate the mode for each Borough.




```python
# Write your function below
# print('mode ',df.mode())
# df4 = df.set_index('BOROUGH')
df3.mode()
# Graded-Funtion Begin (~1 Lines)

# Graded-Funtion End
```




    0    1
    dtype: int64



there is no mode for given data 

From here, we can calculate the Range for each Borough.




```python
# Write your function below
df4= df3.describe()
range = df4['max']-df4['min']
range
# Graded-Funtion Begin (~1 Lines)

# Graded-Funtion End
```




    8185.0



From here, we can calculate the Variance for each Borough.




```python
# Write your function below
df3.var()
# Graded-Funtion Begin (~1 Lines)

# Graded-Funtion End
```




    4799.127995598708



From here, we can calculate the SD for each Borough.




```python
# Write your function below
df3.std()
# Graded-Funtion Begin (~1 Lines)

# Graded-Funtion End
```




    69.27573886721605



Now we can analyze Probability Distibution below.




```python
# for inline plots in jupyter
%matplotlib inline
# import matplotlib
import matplotlib.pyplot as plt
# for latex equations
from IPython.display import Math, Latex
# for displaying images
from IPython.core.display import Image
# import seaborn
import seaborn as sns
# settings for seaborn plotting style
sns.set(color_codes=True)
# settings for seaborn plot sizes
sns.set(rc={'figure.figsize':(5,5)})
```


```python
# Write your function below

# Graded-Funtion Begin
ax = sns.distplot(df1,
                  bins=100,
                  kde=True,
                  color='skyblue',
                  hist_kws={"linewidth": 15,'alpha':1})
ax.set(xlabel='Uniform Distribution ', ylabel='Frequency')




# Graded-Funtion End
```




    [Text(0, 0.5, 'Frequency'), Text(0.5, 0, 'Uniform Distribution ')]




![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/nyc-property/output_55_1.png)


Now we can analyze Confidence Intervals below.




```python
df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>BOROUGH</th>
      <th>NEIGHBORHOOD</th>
      <th>BUILDING CLASS CATEGORY</th>
      <th>TAX CLASS AT PRESENT</th>
      <th>BLOCK</th>
      <th>LOT</th>
      <th>BUILDING CLASS AT PRESENT</th>
      <th>ADDRESS</th>
      <th>APARTMENT NUMBER</th>
      <th>ZIP CODE</th>
      <th>RESIDENTIAL UNITS</th>
      <th>COMMERCIAL UNITS</th>
      <th>TOTAL UNITS</th>
      <th>LAND SQUARE FEET</th>
      <th>GROSS SQUARE FEET</th>
      <th>YEAR BUILT</th>
      <th>TAX CLASS AT TIME OF SALE</th>
      <th>BUILDING CLASS AT TIME OF SALE</th>
      <th>SALE PRICE</th>
      <th>SALE DATE</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>ALPHABET CITY</td>
      <td>07 RENTALS - WALKUP APARTMENTS</td>
      <td>2A</td>
      <td>392</td>
      <td>6</td>
      <td>C2</td>
      <td>153 AVENUE B</td>
      <td></td>
      <td>10009</td>
      <td>5</td>
      <td>0</td>
      <td>5</td>
      <td>1633.0</td>
      <td>6440.0</td>
      <td>1900</td>
      <td>2</td>
      <td>C2</td>
      <td>6625000.0</td>
      <td>2017-07-19</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>ALPHABET CITY</td>
      <td>07 RENTALS - WALKUP APARTMENTS</td>
      <td>2</td>
      <td>399</td>
      <td>26</td>
      <td>C7</td>
      <td>234 EAST 4TH   STREET</td>
      <td></td>
      <td>10009</td>
      <td>28</td>
      <td>3</td>
      <td>31</td>
      <td>4616.0</td>
      <td>18690.0</td>
      <td>1900</td>
      <td>2</td>
      <td>C7</td>
      <td>NaN</td>
      <td>2016-12-14</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1</td>
      <td>ALPHABET CITY</td>
      <td>07 RENTALS - WALKUP APARTMENTS</td>
      <td>2</td>
      <td>399</td>
      <td>39</td>
      <td>C7</td>
      <td>197 EAST 3RD   STREET</td>
      <td></td>
      <td>10009</td>
      <td>16</td>
      <td>1</td>
      <td>17</td>
      <td>2212.0</td>
      <td>7803.0</td>
      <td>1900</td>
      <td>2</td>
      <td>C7</td>
      <td>NaN</td>
      <td>2016-12-09</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1</td>
      <td>ALPHABET CITY</td>
      <td>07 RENTALS - WALKUP APARTMENTS</td>
      <td>2B</td>
      <td>402</td>
      <td>21</td>
      <td>C4</td>
      <td>154 EAST 7TH STREET</td>
      <td></td>
      <td>10009</td>
      <td>10</td>
      <td>0</td>
      <td>10</td>
      <td>2272.0</td>
      <td>6794.0</td>
      <td>1913</td>
      <td>2</td>
      <td>C4</td>
      <td>3936272.0</td>
      <td>2016-09-23</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1</td>
      <td>ALPHABET CITY</td>
      <td>07 RENTALS - WALKUP APARTMENTS</td>
      <td>2A</td>
      <td>404</td>
      <td>55</td>
      <td>C2</td>
      <td>301 EAST 10TH   STREET</td>
      <td></td>
      <td>10009</td>
      <td>6</td>
      <td>0</td>
      <td>6</td>
      <td>2369.0</td>
      <td>4615.0</td>
      <td>1900</td>
      <td>2</td>
      <td>C2</td>
      <td>8000000.0</td>
      <td>2016-11-17</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Write your function below
dx = df[["SALE PRICE", "BOROUGH"]].dropna()
# dx
pd.crosstab(dx['SALE PRICE'], dx['BOROUGH'])
# Graded-Funtion Begin

```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th>BOROUGH</th>
      <th>1</th>
      <th>2</th>
      <th>3</th>
      <th>4</th>
      <th>5</th>
    </tr>
    <tr>
      <th>SALE PRICE</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0.000000e+00</th>
      <td>0</td>
      <td>1826</td>
      <td>8186</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1.000000e+00</th>
      <td>30</td>
      <td>22</td>
      <td>37</td>
      <td>28</td>
      <td>9</td>
    </tr>
    <tr>
      <th>2.000000e+00</th>
      <td>2</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>3.000000e+00</th>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>5.000000e+00</th>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>5.650000e+08</th>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>6.200000e+08</th>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>6.520000e+08</th>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1.040000e+09</th>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2.210000e+09</th>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
<p>10007 rows × 5 columns</p>
</div>




```python
p_fm = 1826/(1826+22)
p_fm
```




    0.9880952380952381




```python
n = 1826+22
n
```




    1848




```python
br_2 = np.sqrt(p_fm * (1 - p_fm) / n)
br_2
```




    0.002522950772404102




```python
z_score = 1.96
lcb = p_fm - z_score* br_2 #lower limit of the CI
ucb = p_fm + z_score* br_2 #upper limit of the CI
print('Confidence interval ',lcb, ucb)
```

    Confidence interval  0.9831502545813261 0.9930402216091502


Make your Hypothesis Testing below




```python
import statsmodels.api as sm
import numpy as np
import matplotlib.pyplot as plt
import pandas as pd
```


```python
df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>BOROUGH</th>
      <th>NEIGHBORHOOD</th>
      <th>BUILDING CLASS CATEGORY</th>
      <th>TAX CLASS AT PRESENT</th>
      <th>BLOCK</th>
      <th>LOT</th>
      <th>BUILDING CLASS AT PRESENT</th>
      <th>ADDRESS</th>
      <th>APARTMENT NUMBER</th>
      <th>ZIP CODE</th>
      <th>RESIDENTIAL UNITS</th>
      <th>COMMERCIAL UNITS</th>
      <th>TOTAL UNITS</th>
      <th>LAND SQUARE FEET</th>
      <th>GROSS SQUARE FEET</th>
      <th>YEAR BUILT</th>
      <th>TAX CLASS AT TIME OF SALE</th>
      <th>BUILDING CLASS AT TIME OF SALE</th>
      <th>SALE PRICE</th>
      <th>SALE DATE</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>ALPHABET CITY</td>
      <td>07 RENTALS - WALKUP APARTMENTS</td>
      <td>2A</td>
      <td>392</td>
      <td>6</td>
      <td>C2</td>
      <td>153 AVENUE B</td>
      <td></td>
      <td>10009</td>
      <td>5</td>
      <td>0</td>
      <td>5</td>
      <td>1633.0</td>
      <td>6440.0</td>
      <td>1900</td>
      <td>2</td>
      <td>C2</td>
      <td>6625000.0</td>
      <td>2017-07-19</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>ALPHABET CITY</td>
      <td>07 RENTALS - WALKUP APARTMENTS</td>
      <td>2</td>
      <td>399</td>
      <td>26</td>
      <td>C7</td>
      <td>234 EAST 4TH   STREET</td>
      <td></td>
      <td>10009</td>
      <td>28</td>
      <td>3</td>
      <td>31</td>
      <td>4616.0</td>
      <td>18690.0</td>
      <td>1900</td>
      <td>2</td>
      <td>C7</td>
      <td>NaN</td>
      <td>2016-12-14</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1</td>
      <td>ALPHABET CITY</td>
      <td>07 RENTALS - WALKUP APARTMENTS</td>
      <td>2</td>
      <td>399</td>
      <td>39</td>
      <td>C7</td>
      <td>197 EAST 3RD   STREET</td>
      <td></td>
      <td>10009</td>
      <td>16</td>
      <td>1</td>
      <td>17</td>
      <td>2212.0</td>
      <td>7803.0</td>
      <td>1900</td>
      <td>2</td>
      <td>C7</td>
      <td>NaN</td>
      <td>2016-12-09</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1</td>
      <td>ALPHABET CITY</td>
      <td>07 RENTALS - WALKUP APARTMENTS</td>
      <td>2B</td>
      <td>402</td>
      <td>21</td>
      <td>C4</td>
      <td>154 EAST 7TH STREET</td>
      <td></td>
      <td>10009</td>
      <td>10</td>
      <td>0</td>
      <td>10</td>
      <td>2272.0</td>
      <td>6794.0</td>
      <td>1913</td>
      <td>2</td>
      <td>C4</td>
      <td>3936272.0</td>
      <td>2016-09-23</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1</td>
      <td>ALPHABET CITY</td>
      <td>07 RENTALS - WALKUP APARTMENTS</td>
      <td>2A</td>
      <td>404</td>
      <td>55</td>
      <td>C2</td>
      <td>301 EAST 10TH   STREET</td>
      <td></td>
      <td>10009</td>
      <td>6</td>
      <td>0</td>
      <td>6</td>
      <td>2369.0</td>
      <td>4615.0</td>
      <td>1900</td>
      <td>2</td>
      <td>C2</td>
      <td>8000000.0</td>
      <td>2016-11-17</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>84543</th>
      <td>5</td>
      <td>WOODROW</td>
      <td>02 TWO FAMILY DWELLINGS</td>
      <td>1</td>
      <td>7349</td>
      <td>34</td>
      <td>B9</td>
      <td>37 QUAIL LANE</td>
      <td></td>
      <td>10309</td>
      <td>2</td>
      <td>0</td>
      <td>2</td>
      <td>2400.0</td>
      <td>2575.0</td>
      <td>1998</td>
      <td>1</td>
      <td>B9</td>
      <td>450000.0</td>
      <td>2016-11-28</td>
    </tr>
    <tr>
      <th>84544</th>
      <td>5</td>
      <td>WOODROW</td>
      <td>02 TWO FAMILY DWELLINGS</td>
      <td>1</td>
      <td>7349</td>
      <td>78</td>
      <td>B9</td>
      <td>32 PHEASANT LANE</td>
      <td></td>
      <td>10309</td>
      <td>2</td>
      <td>0</td>
      <td>2</td>
      <td>2498.0</td>
      <td>2377.0</td>
      <td>1998</td>
      <td>1</td>
      <td>B9</td>
      <td>550000.0</td>
      <td>2017-04-21</td>
    </tr>
    <tr>
      <th>84545</th>
      <td>5</td>
      <td>WOODROW</td>
      <td>02 TWO FAMILY DWELLINGS</td>
      <td>1</td>
      <td>7351</td>
      <td>60</td>
      <td>B2</td>
      <td>49 PITNEY AVENUE</td>
      <td></td>
      <td>10309</td>
      <td>2</td>
      <td>0</td>
      <td>2</td>
      <td>4000.0</td>
      <td>1496.0</td>
      <td>1925</td>
      <td>1</td>
      <td>B2</td>
      <td>460000.0</td>
      <td>2017-07-05</td>
    </tr>
    <tr>
      <th>84546</th>
      <td>5</td>
      <td>WOODROW</td>
      <td>22 STORE BUILDINGS</td>
      <td>4</td>
      <td>7100</td>
      <td>28</td>
      <td>K6</td>
      <td>2730 ARTHUR KILL ROAD</td>
      <td></td>
      <td>10309</td>
      <td>0</td>
      <td>7</td>
      <td>7</td>
      <td>208033.0</td>
      <td>64117.0</td>
      <td>2001</td>
      <td>4</td>
      <td>K6</td>
      <td>11693337.0</td>
      <td>2016-12-21</td>
    </tr>
    <tr>
      <th>84547</th>
      <td>5</td>
      <td>WOODROW</td>
      <td>35 INDOOR PUBLIC AND CULTURAL FACILITIES</td>
      <td>4</td>
      <td>7105</td>
      <td>679</td>
      <td>P9</td>
      <td>155 CLAY PIT ROAD</td>
      <td></td>
      <td>10309</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>10796.0</td>
      <td>2400.0</td>
      <td>2006</td>
      <td>4</td>
      <td>P9</td>
      <td>69300.0</td>
      <td>2016-10-27</td>
    </tr>
  </tbody>
</table>
<p>83783 rows × 20 columns</p>
</div>



Null Hypothesis: sale price BOROUGH1>BOROUGH2\
Alternative Hypthosis: sale price BOROUGH1<BOROUGH2



```python
# Write your function below
b1 = df[df['BOROUGH'] == 1]
b2 = df[df['BOROUGH'] == 2]

# Graded-Funtion Begin
n1 = len(b1)
mu1 = b1["SALE PRICE"].mean()
sd1 = b1["SALE PRICE"].std()

n2 = len(b2)
mu2 = b2["SALE PRICE"].mean()
sd2 = b2["SALE PRICE"].std()

print(n1, mu1, sd1)
print(n2, mu2, sd2)


# Graded-Funtion End
```

    18102 3344641.9823292056 24140481.087110177
    6994 594677.118387189 2793509.0485762



```python
sm.stats.ztest(b1["SALE PRICE"].dropna(), b2["SALE PRICE"].dropna(),alternative='two-sided')
```




    (9.495736658776908, 2.1865980196322893e-21)



Write your final conclusion below. \
because of p-value b2 smaller than b1, then null hypothesis is correct \

