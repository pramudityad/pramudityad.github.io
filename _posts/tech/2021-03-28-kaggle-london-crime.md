---
title: "Analytics London Crime dataset"
categories:
  - Tech
tags:
  - analytics
  - jupyter notebook
---

## Publication-grade Plot Introduction

The aim of this projects is to introduce you to data visualization with Python as concrete and as consistent as possible. Using what you’ve learned; download the London Crime Dataset from Kaggle. This dataset is a record of crime in major metropolitan areas, such as London, occurs in distinct patterns. This data covers the number of criminal reports by month, LSOA borough, and major/minor category from Jan 2008-Dec 2016.

This dataset contains:

* `lsoa_code`: this represents a policing area
* `borough`: the london borough for which the statistic is related
* `major_category`: the major crime category
* `minor_category`: the minor crime category
* `value`: the count of the crime for that particular borough, in that particular month
* `year`: the year of the summary statistic
* `month`: the month of the summary statistic

Formulate a question and derive a statistical hypothesis test to answer the question. You have to demonstrate that you’re able to make decisions using data in a scientific manner. And the important things, Visualized the data. Examples of questions can be:

* What is the change in the number of crime incidents from 2011 to 2016?
* What were the top 3 crimes per borough in 2016?

Please make sure that you have completed the session for this course, namely Advanced Visualization which is part of this Program.

Note: You can take a look at Project Rubric below:

Criteria |	Meet Expectations
---|---
Area Plot |	Mengimplementasikan Area Plot Menggunakan `Matplotlib` Dengan Data Yang Relevan Dan Sesuai Dengan Kegunaan Plot/Grafik
Histogram |	Mengimplementasikan Histogram Menggunakan `Matplotlib` Dengan Data Yang Relevan Dan Sesuai Dengan Kegunaan Plot/Grafik.
Bar Chart | Mengimplementasikan Bar Chart Menggunakan `Matplotlib` Dengan Data Yang Relevan Dan Sesuai Dengan Kegunaan Plot/Grafik.
Pie Chart |	Mengimplementasikan Pie Chart Menggunakan `Matplotlib` Dengan Data Yang Relevan Dan Sesuai Dengan Kegunaan Plot/Grafik.
Box Plot |	Mengimplementasikan Box Plot Menggunakan `Matplotlib` Dengan Data Yang Relevan Dan Sesuai Dengan Kegunaan Plot/Grafik.
Scatter Plot |	Mengimplementasikan Scatter Plot Menggunakan `Matplotlib` Dengan Data Yang Relevan Dan Sesuai Dengan Kegunaan Plot/Grafik.
Word Clouds |	Mengimplementasikan Word Clouds Menggunakan `Wordclouds` Library Dengan Data Yang Relevan Dan Sesuai Dengan Kegunaan Plot/Grafik.
Folium Maps |	Mengimplementasikan London Maps Menggunakan `Folium`.
Preprocessing |	Student Melakukan Preproses Dataset Sebelum Menerapkan Visualisasi. | | Apakah Kode Berjalan Tanpa Ada Eror?
Apakah Kode Berjalan Tanpa Ada Eror? |	Seluruh Kode Berfungsi Dan Dibuat Dengan Benar.
Area Plot |	Menarik Informasi/Kesimpulan Berdasarkan Area Plot Yang Telah Student Buat
Histogram |	Menarik Informasi/Kesimpulan Berdasarkan Histogram Yang Telah Student Buat
Bar Chart |	Menarik Informasi/Kesimpulan Berdasarkan Bar Chart Yang Telah Student Buat
Pie Chart |	Menarik Informasi/Kesimpulan Berdasarkan Pie Chart Yang Telah Student Buat
Box Plot |	Menarik Informasi/Kesimpulan Berdasarkan Box Plot Yang Telah Student Buat
Scatter Plot |	Menarik Informasi/Kesimpulan Berdasarkan Scatter Plot Yang Telah Student Buat
Overall Analysis |	Menarik Informasi/Kesimpulan Dari Keseluruhan Plot Yang Dapat Menjawab Hipotesis.

**Focus on "Graded-Function" sections.**

------------

## Exploring Datasets with *pandas* <a id="0"></a>

*pandas* is an essential data analysis toolkit for Python. From their [website](http://pandas.pydata.org/):
>*pandas* is a Python package providing fast, flexible, and expressive data structures designed to make working with “relational” or “labeled” data both easy and intuitive. It aims to be the fundamental high-level building block for doing practical, **real world** data analysis in Python.

The course heavily relies on *pandas* for data wrangling, analysis, and visualization. We encourage you to spend some time and  familizare yourself with the *pandas* API Reference: http://pandas.pydata.org/pandas-docs/stable/api.html.

The first thing we'll do is import two key data analysis modules: *pandas* and **Numpy**.


```python
import numpy as np
import pandas as pd
```


```python
df = pd.read_csv('london_crime_by_lsoa.csv')

print ('Data read into a pandas dataframe!')
```

    Data read into a pandas dataframe!



```python
# Let's view the top 5 rows of the dataset using the head() function.
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
      <th>lsoa_code</th>
      <th>borough</th>
      <th>major_category</th>
      <th>minor_category</th>
      <th>value</th>
      <th>year</th>
      <th>month</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>E01001116</td>
      <td>Croydon</td>
      <td>Burglary</td>
      <td>Burglary in Other Buildings</td>
      <td>0</td>
      <td>2016</td>
      <td>11</td>
    </tr>
    <tr>
      <th>1</th>
      <td>E01001646</td>
      <td>Greenwich</td>
      <td>Violence Against the Person</td>
      <td>Other violence</td>
      <td>0</td>
      <td>2016</td>
      <td>11</td>
    </tr>
    <tr>
      <th>2</th>
      <td>E01000677</td>
      <td>Bromley</td>
      <td>Violence Against the Person</td>
      <td>Other violence</td>
      <td>0</td>
      <td>2015</td>
      <td>5</td>
    </tr>
    <tr>
      <th>3</th>
      <td>E01003774</td>
      <td>Redbridge</td>
      <td>Burglary</td>
      <td>Burglary in Other Buildings</td>
      <td>0</td>
      <td>2016</td>
      <td>3</td>
    </tr>
    <tr>
      <th>4</th>
      <td>E01004563</td>
      <td>Wandsworth</td>
      <td>Robbery</td>
      <td>Personal Property</td>
      <td>0</td>
      <td>2008</td>
      <td>6</td>
    </tr>
  </tbody>
</table>
</div>




```python
# We can also veiw the bottom 5 rows of the dataset using the tail() function.
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
      <th>lsoa_code</th>
      <th>borough</th>
      <th>major_category</th>
      <th>minor_category</th>
      <th>value</th>
      <th>year</th>
      <th>month</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>13490599</th>
      <td>E01000504</td>
      <td>Brent</td>
      <td>Criminal Damage</td>
      <td>Criminal Damage To Dwelling</td>
      <td>0</td>
      <td>2015</td>
      <td>2</td>
    </tr>
    <tr>
      <th>13490600</th>
      <td>E01002504</td>
      <td>Hillingdon</td>
      <td>Robbery</td>
      <td>Personal Property</td>
      <td>1</td>
      <td>2015</td>
      <td>6</td>
    </tr>
    <tr>
      <th>13490601</th>
      <td>E01004165</td>
      <td>Sutton</td>
      <td>Burglary</td>
      <td>Burglary in a Dwelling</td>
      <td>0</td>
      <td>2011</td>
      <td>2</td>
    </tr>
    <tr>
      <th>13490602</th>
      <td>E01001134</td>
      <td>Croydon</td>
      <td>Robbery</td>
      <td>Business Property</td>
      <td>0</td>
      <td>2011</td>
      <td>5</td>
    </tr>
    <tr>
      <th>13490603</th>
      <td>E01003413</td>
      <td>Merton</td>
      <td>Violence Against the Person</td>
      <td>Wounding/GBH</td>
      <td>0</td>
      <td>2015</td>
      <td>6</td>
    </tr>
  </tbody>
</table>
</div>



When analyzing a dataset, it's always a good idea to start by getting basic information about your dataframe. We can do this by using the `info()` method.


```python
print(df.info())
print(df.describe())
print('minor_category ',df.minor_category.unique())
print('major_category ',df.major_category.unique())
print('borough ',df.borough.unique())

print("To check if any colun has null values")
print(df.isnull().any())
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 13490604 entries, 0 to 13490603
    Data columns (total 7 columns):
     #   Column          Dtype 
    ---  ------          ----- 
     0   lsoa_code       object
     1   borough         object
     2   major_category  object
     3   minor_category  object
     4   value           int64 
     5   year            int64 
     6   month           int64 
    dtypes: int64(3), object(4)
    memory usage: 720.5+ MB
    None
                  value          year         month
    count  1.349060e+07  1.349060e+07  1.349060e+07
    mean   4.779444e-01  2.012000e+03  6.500000e+00
    std    1.771513e+00  2.581989e+00  3.452053e+00
    min    0.000000e+00  2.008000e+03  1.000000e+00
    25%    0.000000e+00  2.010000e+03  3.750000e+00
    50%    0.000000e+00  2.012000e+03  6.500000e+00
    75%    1.000000e+00  2.014000e+03  9.250000e+00
    max    3.090000e+02  2.016000e+03  1.200000e+01
    minor_category  ['Burglary in Other Buildings' 'Other violence' 'Personal Property'
     'Other Theft' 'Offensive Weapon' 'Criminal Damage To Other Building'
     'Theft/Taking of Pedal Cycle' 'Motor Vehicle Interference & Tampering'
     'Theft/Taking Of Motor Vehicle' 'Wounding/GBH' 'Other Theft Person'
     'Common Assault' 'Theft From Shops' 'Possession Of Drugs' 'Harassment'
     'Handling Stolen Goods' 'Criminal Damage To Dwelling'
     'Burglary in a Dwelling' 'Criminal Damage To Motor Vehicle'
     'Other Criminal Damage' 'Counted per Victim' 'Going Equipped'
     'Other Fraud & Forgery' 'Assault with Injury' 'Drug Trafficking'
     'Other Drugs' 'Business Property' 'Other Notifiable' 'Other Sexual'
     'Theft From Motor Vehicle' 'Rape' 'Murder']
    major_category  ['Burglary' 'Violence Against the Person' 'Robbery' 'Theft and Handling'
     'Criminal Damage' 'Drugs' 'Fraud or Forgery' 'Other Notifiable Offences'
     'Sexual Offences']
    borough  ['Croydon' 'Greenwich' 'Bromley' 'Redbridge' 'Wandsworth' 'Ealing'
     'Hounslow' 'Newham' 'Sutton' 'Haringey' 'Lambeth' 'Richmond upon Thames'
     'Hillingdon' 'Havering' 'Barking and Dagenham' 'Kingston upon Thames'
     'Westminster' 'Hackney' 'Enfield' 'Harrow' 'Lewisham' 'Brent' 'Southwark'
     'Barnet' 'Waltham Forest' 'Camden' 'Bexley' 'Kensington and Chelsea'
     'Islington' 'Tower Hamlets' 'Hammersmith and Fulham' 'Merton'
     'City of London']
    To check if any colun has null values
    lsoa_code         False
    borough           False
    major_category    False
    minor_category    False
    value             False
    year              False
    month             False
    dtype: bool


To get the list of column headers we can call upon the dataframe's `.columns` parameter.


```python
df.columns.values
```




    array(['lsoa_code', 'borough', 'major_category', 'minor_category',
           'value', 'year', 'month'], dtype=object)



Similarly, to get the list of indicies we use the `.index` parameter.


```python
df.index.values
```




    array([       0,        1,        2, ..., 13490601, 13490602, 13490603])



Rename column


```python
df.rename(columns={'borough':'District'}, inplace=True)
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
      <th>lsoa_code</th>
      <th>District</th>
      <th>major_category</th>
      <th>minor_category</th>
      <th>value</th>
      <th>year</th>
      <th>month</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>E01001116</td>
      <td>Croydon</td>
      <td>Burglary</td>
      <td>Burglary in Other Buildings</td>
      <td>0</td>
      <td>2016</td>
      <td>11</td>
    </tr>
    <tr>
      <th>1</th>
      <td>E01001646</td>
      <td>Greenwich</td>
      <td>Violence Against the Person</td>
      <td>Other violence</td>
      <td>0</td>
      <td>2016</td>
      <td>11</td>
    </tr>
    <tr>
      <th>2</th>
      <td>E01000677</td>
      <td>Bromley</td>
      <td>Violence Against the Person</td>
      <td>Other violence</td>
      <td>0</td>
      <td>2015</td>
      <td>5</td>
    </tr>
    <tr>
      <th>3</th>
      <td>E01003774</td>
      <td>Redbridge</td>
      <td>Burglary</td>
      <td>Burglary in Other Buildings</td>
      <td>0</td>
      <td>2016</td>
      <td>3</td>
    </tr>
    <tr>
      <th>4</th>
      <td>E01004563</td>
      <td>Wandsworth</td>
      <td>Robbery</td>
      <td>Personal Property</td>
      <td>0</td>
      <td>2008</td>
      <td>6</td>
    </tr>
  </tbody>
</table>
</div>



To view the dimensions of the dataframe, we use the `.shape` parameter.


```python
print(df.shape)
```

    (13490604, 7)


Let's make one dataset that contains value 1 in value features.


```python
criminal = df[df['value'] == 1]
```


```python
df1 = df.copy()
df1.drop(['lsoa_code','minor_category'], axis=1, inplace=True)
df1
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
      <th>District</th>
      <th>major_category</th>
      <th>value</th>
      <th>year</th>
      <th>month</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Croydon</td>
      <td>Burglary</td>
      <td>0</td>
      <td>2016</td>
      <td>11</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Greenwich</td>
      <td>Violence Against the Person</td>
      <td>0</td>
      <td>2016</td>
      <td>11</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Bromley</td>
      <td>Violence Against the Person</td>
      <td>0</td>
      <td>2015</td>
      <td>5</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Redbridge</td>
      <td>Burglary</td>
      <td>0</td>
      <td>2016</td>
      <td>3</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Wandsworth</td>
      <td>Robbery</td>
      <td>0</td>
      <td>2008</td>
      <td>6</td>
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
      <th>13490599</th>
      <td>Brent</td>
      <td>Criminal Damage</td>
      <td>0</td>
      <td>2015</td>
      <td>2</td>
    </tr>
    <tr>
      <th>13490600</th>
      <td>Hillingdon</td>
      <td>Robbery</td>
      <td>1</td>
      <td>2015</td>
      <td>6</td>
    </tr>
    <tr>
      <th>13490601</th>
      <td>Sutton</td>
      <td>Burglary</td>
      <td>0</td>
      <td>2011</td>
      <td>2</td>
    </tr>
    <tr>
      <th>13490602</th>
      <td>Croydon</td>
      <td>Robbery</td>
      <td>0</td>
      <td>2011</td>
      <td>5</td>
    </tr>
    <tr>
      <th>13490603</th>
      <td>Merton</td>
      <td>Violence Against the Person</td>
      <td>0</td>
      <td>2015</td>
      <td>6</td>
    </tr>
  </tbody>
</table>
<p>13490604 rows × 5 columns</p>
</div>




```python
drugs = df1[(df1['major_category'] == 'Drugs') & (df1['year'] == 2016)]
print(drugs.value.sum())
```

    38914



```python
df_sum = df1.groupby(['year','District']).size().reset_index(name='count_per_year')
print(df_sum)
print(df_sum.columns)
```

         year              District  count_per_year
    0    2008  Barking and Dagenham           34560
    1    2008                Barnet           63648
    2    2008                Bexley           42852
    3    2008                 Brent           54516
    4    2008               Bromley           58212
    ..    ...                   ...             ...
    292  2016                Sutton           35832
    293  2016         Tower Hamlets           45792
    294  2016        Waltham Forest           45144
    295  2016            Wandsworth           55404
    296  2016           Westminster           40740
    
    [297 rows x 3 columns]
    Index(['year', 'District', 'count_per_year'], dtype='object')



```python
table = df1.pivot_table(values='value', index=['year'],columns=['major_category'], aggfunc=np.sum, fill_value=0)
table
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
      <th>major_category</th>
      <th>Burglary</th>
      <th>Criminal Damage</th>
      <th>Drugs</th>
      <th>Fraud or Forgery</th>
      <th>Other Notifiable Offences</th>
      <th>Robbery</th>
      <th>Sexual Offences</th>
      <th>Theft and Handling</th>
      <th>Violence Against the Person</th>
    </tr>
    <tr>
      <th>year</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2008</th>
      <td>88092</td>
      <td>91872</td>
      <td>68804</td>
      <td>5325</td>
      <td>10112</td>
      <td>29627</td>
      <td>1273</td>
      <td>283692</td>
      <td>159844</td>
    </tr>
    <tr>
      <th>2009</th>
      <td>90619</td>
      <td>85565</td>
      <td>60549</td>
      <td>0</td>
      <td>10644</td>
      <td>29568</td>
      <td>0</td>
      <td>279492</td>
      <td>160777</td>
    </tr>
    <tr>
      <th>2010</th>
      <td>86826</td>
      <td>77897</td>
      <td>58674</td>
      <td>0</td>
      <td>10768</td>
      <td>32341</td>
      <td>0</td>
      <td>290924</td>
      <td>157894</td>
    </tr>
    <tr>
      <th>2011</th>
      <td>93315</td>
      <td>70914</td>
      <td>57550</td>
      <td>0</td>
      <td>10264</td>
      <td>36679</td>
      <td>0</td>
      <td>309292</td>
      <td>146901</td>
    </tr>
    <tr>
      <th>2012</th>
      <td>93392</td>
      <td>62158</td>
      <td>51776</td>
      <td>0</td>
      <td>10675</td>
      <td>35260</td>
      <td>0</td>
      <td>334054</td>
      <td>150014</td>
    </tr>
    <tr>
      <th>2013</th>
      <td>87222</td>
      <td>56206</td>
      <td>50278</td>
      <td>0</td>
      <td>10811</td>
      <td>29337</td>
      <td>0</td>
      <td>306372</td>
      <td>146181</td>
    </tr>
    <tr>
      <th>2014</th>
      <td>76053</td>
      <td>59279</td>
      <td>44435</td>
      <td>0</td>
      <td>13037</td>
      <td>22150</td>
      <td>0</td>
      <td>279880</td>
      <td>185349</td>
    </tr>
    <tr>
      <th>2015</th>
      <td>70489</td>
      <td>62976</td>
      <td>39785</td>
      <td>0</td>
      <td>14229</td>
      <td>21383</td>
      <td>0</td>
      <td>284022</td>
      <td>218740</td>
    </tr>
    <tr>
      <th>2016</th>
      <td>68285</td>
      <td>64071</td>
      <td>38914</td>
      <td>0</td>
      <td>15809</td>
      <td>22528</td>
      <td>0</td>
      <td>294133</td>
      <td>232381</td>
    </tr>
  </tbody>
</table>
</div>



## Visualizing Data using Matplotlib<a id="8"></a>

### Matplotlib: Standard Python Visualization Library<a id="10"></a>

The primary plotting library we will explore in the course is [Matplotlib](http://matplotlib.org/).  As mentioned on their website: 
>Matplotlib is a Python 2D plotting library which produces publication quality figures in a variety of hardcopy formats and interactive environments across platforms. Matplotlib can be used in Python scripts, the Python and IPython shell, the jupyter notebook, web application servers, and four graphical user interface toolkits.

If you are aspiring to create impactful visualization with python, Matplotlib is an essential tool to have at your disposal.

**Matplotlib.Pyplot**

One of the core aspects of Matplotlib is `matplotlib.pyplot`.

Let's start by importing `Matplotlib` and `Matplotlib.pyplot` as follows:


```python
# we are using the inline backend
%matplotlib inline 

import matplotlib as mpl
import matplotlib.pyplot as plt
```


```python
mpl.style.use(['ggplot']) # optional: for ggplot-like style
```

## Area Pots (Series/Dataframe) <a id="12"></a>

**What is a line plot and why use it?**

An Area chart or area plot is a type of plot which displays information as a series of data points called 'markers' connected by straight line segments. It is a basic type of chart common in many fields. Use line plot when you have a continuous data set. These are best suited for trend-based visualizations of data over a period of time.

**Questions:**
1. what most major_criminal in 2008-2016?


```python
# Write your function below
table.plot(kind='area', 
           alpha=0.45,
             stacked=False,
             figsize=(20, 10), # pass a tuple (x, y) size
             )
# Graded-Funtion Begin (~1 Lines)

# Graded-Funtion End

plt.title('Area Plot of major_criminal in 2008-2016') # add a title to the area plot
plt.ylabel('Number of accident') # add y-label
plt.xlabel('Year') # add x-label

plt.show()
```


![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/london-dataset/output_29_0.png)


**Insight:**

Based on graph, Thef and Handling is most major criminal happend during 2008-2018

## Histogram

A histogram is a way of representing the frequency distribution of numeric dataset. The way it works is it partitions the x-axis into bins, assigns each data point in our dataset to a bin, and then counts the number of data points that have been assigned to each bin. So the y-axis is the frequency or the number of data points in each bin. Note that we can change the bin size and usually one needs to tweak it so that the distribution is displayed nicely.

**Question:**
1. Frequency major case criminal in London
(Make your own questions)


```python
# Write your function below
count, bin_edges = np.histogram(table, 15)
table.plot(kind ='hist', 
          figsize=(15, 6),
          bins=15,
          alpha=0.6,
          xticks=bin_edges,         
         )
# Graded-Funtion Begin (~2 Lines)

# Graded-Funtion End

plt.title('Histogram of major criminal in 2008-2016') # add a title to the histogram
plt.ylabel('Number of Years') # add y-label
plt.xlabel('Number of Criminal ') # add x-label

plt.show()
```


![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/london-dataset/output_32_0.png)


**Insight:**
Most frequency cases in london between 2008-2016 is Thef and Handling
(Make your own Insight)

## Bar Charts (Dataframe) <a id="10"></a>

A bar plot is a way of representing data where the *length* of the bars represents the magnitude/size of the feature/variable. Bar graphs usually represent numerical and categorical variables grouped in intervals. 

To create a bar plot, we can pass one of two arguments via `kind` parameter in `plot()`:

* `kind=bar` creates a *vertical* bar plot
* `kind=barh` creates a *horizontal* bar plot

**Question:**
1. Yearly drug case in London from 2008-2016?


```python
# Write your function below
table_bar = table['Drugs']
table_bar.plot(kind='bar', figsize=(10, 6))
# Graded-Funtion Begin (~1 Lines)

# Graded-Funtion End

plt.xlabel('Year') # add to x-label to the plot
plt.ylabel('Number of crime drugs') # add y-label to the plot
plt.title('London drugs case in 2008-2016') # add title to the plot

plt.show()
```


![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/london-dataset/output_35_0.png)


**Insight:**

Drug case in London is decrasing in 2008-2016

## Pie Charts <a id="6"></a>

A `pie chart` is a circualr graphic that displays numeric proportions by dividing a circle (or pie) into proportional slices. You are most likely already familiar with pie charts as it is widely used in business and media. We can create pie charts in Matplotlib by passing in the `kind=pie` keyword.

**Question:**

(Make your own questions)


```python
table_pie = table.transpose()
table_pie['total'] = table.sum()
table_pie

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
      <th>year</th>
      <th>2008</th>
      <th>2009</th>
      <th>2010</th>
      <th>2011</th>
      <th>2012</th>
      <th>2013</th>
      <th>2014</th>
      <th>2015</th>
      <th>2016</th>
      <th>total</th>
    </tr>
    <tr>
      <th>major_category</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Burglary</th>
      <td>88092</td>
      <td>90619</td>
      <td>86826</td>
      <td>93315</td>
      <td>93392</td>
      <td>87222</td>
      <td>76053</td>
      <td>70489</td>
      <td>68285</td>
      <td>754293</td>
    </tr>
    <tr>
      <th>Criminal Damage</th>
      <td>91872</td>
      <td>85565</td>
      <td>77897</td>
      <td>70914</td>
      <td>62158</td>
      <td>56206</td>
      <td>59279</td>
      <td>62976</td>
      <td>64071</td>
      <td>630938</td>
    </tr>
    <tr>
      <th>Drugs</th>
      <td>68804</td>
      <td>60549</td>
      <td>58674</td>
      <td>57550</td>
      <td>51776</td>
      <td>50278</td>
      <td>44435</td>
      <td>39785</td>
      <td>38914</td>
      <td>470765</td>
    </tr>
    <tr>
      <th>Fraud or Forgery</th>
      <td>5325</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>5325</td>
    </tr>
    <tr>
      <th>Other Notifiable Offences</th>
      <td>10112</td>
      <td>10644</td>
      <td>10768</td>
      <td>10264</td>
      <td>10675</td>
      <td>10811</td>
      <td>13037</td>
      <td>14229</td>
      <td>15809</td>
      <td>106349</td>
    </tr>
    <tr>
      <th>Robbery</th>
      <td>29627</td>
      <td>29568</td>
      <td>32341</td>
      <td>36679</td>
      <td>35260</td>
      <td>29337</td>
      <td>22150</td>
      <td>21383</td>
      <td>22528</td>
      <td>258873</td>
    </tr>
    <tr>
      <th>Sexual Offences</th>
      <td>1273</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1273</td>
    </tr>
    <tr>
      <th>Theft and Handling</th>
      <td>283692</td>
      <td>279492</td>
      <td>290924</td>
      <td>309292</td>
      <td>334054</td>
      <td>306372</td>
      <td>279880</td>
      <td>284022</td>
      <td>294133</td>
      <td>2661861</td>
    </tr>
    <tr>
      <th>Violence Against the Person</th>
      <td>159844</td>
      <td>160777</td>
      <td>157894</td>
      <td>146901</td>
      <td>150014</td>
      <td>146181</td>
      <td>185349</td>
      <td>218740</td>
      <td>232381</td>
      <td>1558081</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Write your function below

# ratio for each continent with which to offset each wedge.
colors_list = ['gold', 'yellowgreen', 'lightcoral', 'lightskyblue', 'lightgreen', 'pink','red','purple','brown']
explode_list = [0.1, 0, 0, 0, 0, 0, 0, 0.1, 0.1]

# Graded-Funtion Begin (~8 Lines)
table_pie['total'].plot(kind='pie',
                      figsize=(15, 6),
                      autopct='%1.1f%%',
                      startangle=90,
                      shadow=True,
                      labels=None,         # turn off labels on pie chart
                      colors=colors_list,  # add custom colors
                      # the ratio between the center of each pie slice and the start of the text generated by autopct
                      pctdistance=1.12,
                      explode=explode_list  # 'explode'
                      )
# Graded-Funtion End

# scale the title up by 12% to match pctdistance
plt.title('Major criminal case in London (2008-2016)', y=1.12)

plt.axis('equal')

# add legend
plt.legend(labels=table_pie.index, loc='upper left')

plt.show()
```


![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/london-dataset/output_39_0.png)


**Insight:**

Theft and Handling is most major criminal case in London during 2008-2016, with precentage is 41,3%

## Box Plots <a id="8"></a>

A `box plot` is a way of statistically representing the *distribution* of the data through five main dimensions: 

- **Minimun:** Smallest number in the dataset.
- **First quartile:** Middle number between the `minimum` and the `median`.
- **Second quartile (Median):** Middle number of the (sorted) dataset.
- **Third quartile:** Middle number between `median` and `maximum`.
- **Maximum:** Highest number in the dataset.

**Question:**
1. Describe drug case in London from 2008-2016?
(Make your own questions)


```python
# Write your function below
table_bar.plot(kind='box', figsize=(8, 6))
# Graded-Funtion Begin (~1 Lines)

# Graded-Funtion End

plt.title('Box plot of drugs case in London from 1980 - 2013')
plt.ylabel('Number of cases')

plt.show()
```


![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/london-dataset/output_42_0.png)


**Insight:**
Drugs max cases is around 70000 cases
(Make your own Insight)

## Scatter Plots <a id="10"></a>

A `scatter plot` (2D) is a useful method of comparing variables against each other. `Scatter` plots look similar to `line plots` in that they both map independent and dependent variables on a 2D graph. While the datapoints are connected together by a line in a line plot, they are not connected in a scatter plot. The data in a scatter plot is considered to express a trend. With further analysis using tools like regression, we can mathematically calculate this relationship and use it to predict trends outside the dataset.

**Question:**
1. lets compare Drugs and Robbery

(Make your own questions)


```python
table_scatter = table[['Drugs','Robbery']]
table_scatter = table_scatter.reset_index()
table_scatter
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
      <th>major_category</th>
      <th>year</th>
      <th>Drugs</th>
      <th>Robbery</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2008</td>
      <td>68804</td>
      <td>29627</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2009</td>
      <td>60549</td>
      <td>29568</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2010</td>
      <td>58674</td>
      <td>32341</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2011</td>
      <td>57550</td>
      <td>36679</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2012</td>
      <td>51776</td>
      <td>35260</td>
    </tr>
    <tr>
      <th>5</th>
      <td>2013</td>
      <td>50278</td>
      <td>29337</td>
    </tr>
    <tr>
      <th>6</th>
      <td>2014</td>
      <td>44435</td>
      <td>22150</td>
    </tr>
    <tr>
      <th>7</th>
      <td>2015</td>
      <td>39785</td>
      <td>21383</td>
    </tr>
    <tr>
      <th>8</th>
      <td>2016</td>
      <td>38914</td>
      <td>22528</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Write your function below

# Graded-Funtion Begin (~1 Lines)
ax1 = table_scatter.plot(kind='scatter', x='year', y='Drugs', figsize=(10, 6), color='darkblue', label='Drugs')
ax2 = table_scatter.plot(kind='scatter', x='year', y='Robbery', figsize=(10, 6), color='red',label='Robbery', ax=ax1 )

# Graded-Funtion End

plt.title('Comparation Drugs and Robbery cases in London from 2008-2016')
plt.xlabel('Year')
plt.ylabel('Numer of cases')
plt.show()
```


![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/london-dataset/output_46_0.png)


## Word Clouds <a id="8"></a>


`Word` clouds (also known as text clouds or tag clouds) work in a simple way: the more a specific word appears in a source of textual data (such as a speech, blog post, or database), the bigger and bolder it appears in the word cloud.


```python
# install wordcloud
# !conda install -c conda-forge wordcloud --yes

# !pip install wordcloud

# import package and its set of stopwords
from wordcloud import WordCloud, STOPWORDS

print ('Wordcloud is installed and imported!')
```

    Wordcloud is installed and imported!



```python
stopwords = set(STOPWORDS)
```


```python
# table_minor = df[['minor_category']]
source_dataset = ' '.join(df.major_category)
```


```python
# instantiate a word cloud object
your_wordcloud = WordCloud(
    background_color='white',
    max_words=2000,
    stopwords=stopwords
)

# generate the word cloud
your_wordcloud.generate(source_dataset)
```




    <wordcloud.wordcloud.WordCloud at 0x7fc1e4e41850>




```python
# Write your function below

# Graded-Funtion Begin (~1 Lines)
plt.imshow(your_wordcloud, interpolation='bilinear')
# Graded-Funtion End

plt.axis('off')
plt.show()
```


![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/london-dataset/output_52_0.png)
