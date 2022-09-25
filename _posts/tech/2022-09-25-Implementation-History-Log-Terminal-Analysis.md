---

title: "Implementation : History Log Terminal Analysis"

categories:

- Tech

tags:

- data-science

- log system

---

  

# Implementation

  

In this study we focused on how the input data being processed and finally came as output. The input data is unstructured and the expected output is structured data. Given the Design system above using ETL in this implementation phase divided into 2 main parts : backend and frontend. Before that will be describe each technology stack use in this implementation.

  

  

## Stack Use

  

### Mysql 

  

The database served is MySQL. MySQL is an open-source relational database management system. Its name is a combination of "My", the name of co-founder Michael Widenius's daughter My, and "SQL", the abbreviation for Structured Query Language. The database is play role as Load in ETL

  

  

### Jupyter Notebook

  

Jupyter Notebook (formerly IPython Notebook) is a [web-based interactive](https://en.wikipedia.org/wiki/Web_application) computational environment for creating [notebook](https://en.wikipedia.org/wiki/Notebook_interface) documents. Jupyter Notebook is built using several [open-source](https://en.wikipedia.org/wiki/Open-source_software) libraries, including [IPython](https://en.wikipedia.org/wiki/IPython), [ZeroMQ](https://en.wikipedia.org/wiki/ZeroMQ), [Tornado](https://en.wikipedia.org/wiki/Tornado_(web_server)), [jQuery](https://en.wikipedia.org/wiki/JQuery), [Bootstrap](https://en.wikipedia.org/wiki/Bootstrap_(front-end_framework)), and [MathJax](https://en.wikipedia.org/wiki/MathJax). A Jupyter Notebook document is a browser-based [REPL](https://en.wikipedia.org/wiki/Read%E2%80%93eval%E2%80%93print_loop) containing an ordered list of input/output cells which can contain code, text (using [Markdown](https://en.wikipedia.org/wiki/Markdown)), mathematics, [plots](https://en.wikipedia.org/wiki/Plot_(graphics)) and [rich media](https://en.wikipedia.org/wiki/Interactive_media). Underneath the interface, a notebook is a [JSON](https://en.wikipedia.org/wiki/JSON) document, following a versioned schema, usually ending with the ".ipynb" extension. The Jupyter Notebook role is as Extract and Transform in ETL

  

  

### Metabase

  

Metabase is the easy, open-source way to help everyone in your company work with data like an analyst. Its like a database client Interface but with extra intelligence that can visualize the dataset. 

  

## Codebase Explanation

  

  

For the full code you can find in Github repository : https://github.com/pramudityad/assignment-data-science-unstructured

  

### ETL Backend

  

```python

import pandas as pd

pd.options.display.max_columns = None

import re

import os

import time

from tqdm import tqdm

  

for dirname, _, filenames in os.walk('/home/damar.pramuditya/School/Data Science/unstructure_assignment/input/archive'):

for filename in filenames:

print(os.path.join(dirname, filename))

```

  

/home/damar.pramuditya/School/Data Science/unstructure_assignment/input/archive/client_hostname.csv

/home/damar.pramuditya/School/Data Science/unstructure_assignment/input/archive/access.log

  
  
  

```python

!ls -lsSh 'input/archive/access.log'

```

  

3,3G -rw-rw-r-- 1 damar.pramuditya damar.pramuditya 3,3G Feb 13 2021 input/archive/access.log

  
  
  

```python

!head -n 4 'input/archive/access.log'

```

  

54.36.149.41 - - [22/Jan/2019:03:56:14 +0330] "GET /filter/27|13%20%D9%85%DA%AF%D8%A7%D9%BE%DB%8C%DA%A9%D8%B3%D9%84,27|%DA%A9%D9%85%D8%AA%D8%B1%20%D8%A7%D8%B2%205%20%D9%85%DA%AF%D8%A7%D9%BE%DB%8C%DA%A9%D8%B3%D9%84,p53 HTTP/1.1" 200 30577 "-" "Mozilla/5.0 (compatible; AhrefsBot/6.1; +http://ahrefs.com/robot/)" "-"

31.56.96.51 - - [22/Jan/2019:03:56:16 +0330] "GET /image/60844/productModel/200x200 HTTP/1.1" 200 5667 "https://www.zanbil.ir/m/filter/b113" "Mozilla/5.0 (Linux; Android 6.0; ALE-L21 Build/HuaweiALE-L21) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/66.0.3359.158 Mobile Safari/537.36" "-"

31.56.96.51 - - [22/Jan/2019:03:56:16 +0330] "GET /image/61474/productModel/200x200 HTTP/1.1" 200 5379 "https://www.zanbil.ir/m/filter/b113" "Mozilla/5.0 (Linux; Android 6.0; ALE-L21 Build/HuaweiALE-L21) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/66.0.3359.158 Mobile Safari/537.36" "-"

40.77.167.129 - - [22/Jan/2019:03:56:17 +0330] "GET /image/14925/productModel/100x100 HTTP/1.1" 200 1696 "-" "Mozilla/5.0 (compatible; bingbot/2.0; +http://www.bing.com/bingbot.htm)" "-"

  
  
  

```python

# Log Format

  

# This approach assumes the common log format and/or the combined one, which are two of the most commonly used. Eventually other formats can be incorporated. We start with the below regular express taken from:

  

# Regular Expressions Cookbook

# by Jan Goyvaerts, Steven Levithan

# Publisher: O'Reilly Media, Inc. Release Date: August 2012

  

combined_regex = '^(?P<client>\S+) \S+ (?P<userid>\S+) \[(?P<datetime>[^\]]+)\] "(?P<method>[A-Z]+) (?P<request>[^ "]+)? HTTP/[0-9.]+" (?P<status>[0-9]{3}) (?P<size>[0-9]+|-) "(?P<referrer>[^"]*)" "(?P<useragent>[^"]*)'

columns = ['client', 'userid', 'datetime', 'method', 'request', 'status', 'size', 'referer', 'user_agent']

```

  
  

```python

# The Approach

  

# Loop through the lines of the input log file one by one. This ensures minimal memory consumption.

# For each line, check it against the regular expression, and process it:

# Match: append the matched line to a parsed_lines list

# No match: append the non-matching line to the errors_file

# Once parsed_lines reaches 250,000 elements, convert the list to a DataFrame and save it to a parquet file in the output_dir. Clear the list. This also ensures minimal memory usage, and the 250k can be tweaked if necessary.

# Read all the files of the output_dir with read_parquet into a pandas DataFrame. This function handles reading all the files and combines them.

# Optimize the columns by using more efficient data types, most notably the pandas categorical type.

# Write the DataFrame to a single file, for more convenient handling, and with the more efficient datatypes. This results in even faster reading.

# Delete the files in output_dir.

# Read in the final file with read_parquet.

# Start analyzing.

```

  
  

```python

import time

import re

import pandas as pd

  

def logs_to_df(logfile, output_dir, errors_file):

with open(logfile) as source_file:

linenumber = 0

parsed_lines = []

for line in tqdm(source_file):

try:

log_line = re.findall(combined_regex, line)[0]

parsed_lines.append(log_line)

except Exception as e:

with open(errors_file, 'at') as errfile:

print((line, str(e)), file=errfile)

continue

linenumber += 1

if linenumber % 250_000 == 0:

df = pd.DataFrame(parsed_lines, columns=columns)

df.to_parquet(f'{output_dir}/file_{linenumber}.parquet')

parsed_lines.clear()

else:

df = pd.DataFrame(parsed_lines, columns=columns)

df.to_parquet(f'{output_dir}/file_{linenumber}.parquet')

parsed_lines.clear()

```

  
  

```python

%%capture

!pip install advertools

```

  
  

```python

# Times will vary from system to system

%time logs_to_df(logfile='input/archive/access.log', output_dir='output/', errors_file='errors.txt')

```

  

10365152it [01:09, 149847.68it/s]

  
  

CPU times: user 49.8 s, sys: 4.89 s, total: 54.7 s

Wall time: 1min 9s

  
  
  

```python

# check errors

!wc errors.txt

```

  

589 14416 461229 errors.txt

  
  
  

```python

%time logs_df = pd.read_parquet('output/')

```

  

CPU times: user 7.65 s, sys: 1.96 s, total: 9.61 s

Wall time: 4.72 s

  
  
  

```python

# Reading the whole directory takes about 7 seconds.

```

  
  

```python

!du -sh output/

```

  

257M output/

  
  
  

```python

# 257 ÷ 3,300 = 0.07.

  

# The resulting file is 7% the size of the original.

  

# Let's see how much memory it takes:

  

```

  
  

```python

val = 257/3300

print(val)

```

  

0.07787878787878788

  
  
  

```python

logs_df.info(show_counts=True, verbose=True)

```

  

<class 'pandas.core.frame.DataFrame'>

RangeIndex: 10364865 entries, 0 to 10364864

Data columns (total 9 columns):

# Column Non-Null Count Dtype

--- ------ -------------- -----

0 client 10364865 non-null object

1 userid 10364865 non-null object

2 datetime 10364865 non-null object

3 method 10364865 non-null object

4 request 10364865 non-null object

5 status 10364865 non-null object

6 size 10364865 non-null object

7 referer 10364865 non-null object

8 user_agent 10364865 non-null object

dtypes: object(9)

memory usage: 711.7+ MB

  
  
  

```python

# 711 MB. We now remove the files in output dir and optimize the datatypes and use more efficient ones.

# %rm -r output/

```

  
  

```python

# optimized by changing the type data and take out userid field

```

  
  

```python

logs_df['client'] = logs_df['client'].astype('category')

del logs_df['userid']

logs_df['datetime'] = pd.to_datetime(logs_df['datetime'], format='%d/%b/%Y:%H:%M:%S %z')

logs_df['method'] = logs_df['method'].astype('category')

logs_df['status'] = logs_df['status'].astype('int16')

logs_df['size'] = logs_df['size'].astype('int32')

logs_df['referer'] = logs_df['referer'].astype('category')

logs_df['user_agent'] = logs_df['user_agent'].astype('category')

```

  
  

```python

logs_df.info(verbose=True, show_counts=True)

```

  

<class 'pandas.core.frame.DataFrame'>

RangeIndex: 10364865 entries, 0 to 10364864

Data columns (total 8 columns):

# Column Non-Null Count Dtype

--- ------ -------------- -----

0 client 10364865 non-null category

1 datetime 10364865 non-null datetime64[ns, pytz.FixedOffset(210)]

2 method 10364865 non-null category

3 request 10364865 non-null object

4 status 10364865 non-null int16

5 size 10364865 non-null int32

6 referer 10364865 non-null category

7 user_agent 10364865 non-null category

dtypes: category(4), datetime64[ns, pytz.FixedOffset(210)](1), int16(1), int32(1), object(1)

memory usage: 342.3+ MB

  
  
  

```python

# The file was reduced further from 711 to 342 MB. (342 ÷ 711 = 0.48 of the original size)

  

# We now save it to a single file, and read again.

```

  
  

```python

%time logs_df.to_parquet('logs_df.parquet')

```

  

CPU times: user 3.7 s, sys: 891 ms, total: 4.59 s

Wall time: 4.33 s

  
  
  

```python

# Now reading the file took almost half the previous time.

%time logs_df = pd.read_parquet('logs_df.parquet')

```

  

CPU times: user 3.26 s, sys: 1.5 s, total: 4.77 s

Wall time: 3.41 s

  
  
  

```python

logs_df.shape

```

  
  
  
  

(10364865, 8)

  
  
  
  

```python

logs_df

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

<th>client</th>

<th>datetime</th>

<th>method</th>

<th>request</th>

<th>status</th>

<th>size</th>

<th>referer</th>

<th>user_agent</th>

</tr>

</thead>

<tbody>

<tr>

<th>0</th>

<td>37.152.163.59</td>

<td>2019-01-22 12:38:27+03:30</td>

<td>GET</td>

<td>/image/29314?name=%D8%AF%DB%8C%D8%A8%D8%A7-7.j...</td>

<td>200</td>

<td>1105</td>

<td>https://www.zanbil.ir/product/29314/%DA%A9%D8%...</td>

<td>Mozilla/5.0 (Windows NT 6.1; WOW64; Trident/7....</td>

</tr>

<tr>

<th>1</th>

<td>37.152.163.59</td>

<td>2019-01-22 12:38:27+03:30</td>

<td>GET</td>

<td>/static/images/zanbil-kharid.png</td>

<td>200</td>

<td>358</td>

<td>https://www.zanbil.ir/product/29314/%DA%A9%D8%...</td>

<td>Mozilla/5.0 (Windows NT 6.1; WOW64; Trident/7....</td>

</tr>

<tr>

<th>2</th>

<td>85.9.73.119</td>

<td>2019-01-22 12:38:27+03:30</td>

<td>GET</td>

<td>/static/images/next.png</td>

<td>200</td>

<td>3045</td>

<td>https://znbl.ir/static/bundle-bundle_site_head...</td>

<td>Mozilla/5.0 (Windows NT 6.1; Win64; x64) Apple...</td>

</tr>

<tr>

<th>3</th>

<td>37.152.163.59</td>

<td>2019-01-22 12:38:27+03:30</td>

<td>GET</td>

<td>/image/29314?name=%D8%AF%DB%8C%D8%A8%D8%A7-4.j...</td>

<td>200</td>

<td>1457</td>

<td>https://www.zanbil.ir/product/29314/%DA%A9%D8%...</td>

<td>Mozilla/5.0 (Windows NT 6.1; WOW64; Trident/7....</td>

</tr>

<tr>

<th>4</th>

<td>85.9.73.119</td>

<td>2019-01-22 12:38:27+03:30</td>

<td>GET</td>

<td>/static/images/checked.png</td>

<td>200</td>

<td>1083</td>

<td>https://znbl.ir/static/bundle-bundle_site_head...</td>

<td>Mozilla/5.0 (Windows NT 6.1; Win64; x64) Apple...</td>

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

</tr>

<tr>

<th>10364860</th>

<td>86.104.110.254</td>

<td>2019-01-26 16:01:31+03:30</td>

<td>GET</td>

<td>/settings/logo</td>

<td>200</td>

<td>4120</td>

<td>https://www.zanbil.ir/m/browse/tv/%D8%AA%D9%84...</td>

<td>Mozilla/5.0 (iPhone; CPU iPhone OS 12_1 like M...</td>

</tr>

<tr>

<th>10364861</th>

<td>5.125.254.169</td>

<td>2019-01-26 16:01:31+03:30</td>

<td>GET</td>

<td>/image/5/brand</td>

<td>200</td>

<td>2171</td>

<td>https://www.zanbil.ir/m/filter/p62%2Cstexists</td>

<td>Mozilla/5.0 (iPhone; CPU iPhone OS 12_1_2 like...</td>

</tr>

<tr>

<th>10364862</th>

<td>65.49.68.192</td>

<td>2019-01-26 16:01:31+03:30</td>

<td>GET</td>

<td>/image/64646/productModel/150x150</td>

<td>200</td>

<td>5318</td>

<td>https://www.zanbil.ir/browse/audio-and-video-e...</td>

<td>Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:6...</td>

</tr>

<tr>

<th>10364863</th>

<td>5.125.254.169</td>

<td>2019-01-26 16:01:31+03:30</td>

<td>GET</td>

<td>/image/1/brand</td>

<td>200</td>

<td>3924</td>

<td>https://www.zanbil.ir/m/filter/p62%2Cstexists</td>

<td>Mozilla/5.0 (iPhone; CPU iPhone OS 12_1_2 like...</td>

</tr>

<tr>

<th>10364864</th>

<td>65.49.68.192</td>

<td>2019-01-26 16:01:31+03:30</td>

<td>GET</td>

<td>/image/56698/productModel/150x150</td>

<td>200</td>

<td>3570</td>

<td>https://www.zanbil.ir/browse/audio-and-video-e...</td>

<td>Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:6...</td>

</tr>

</tbody>

</table>

<p>10364865 rows × 8 columns</p>

</div>

  
  
  
  

```python

!pip install SQLAlchemy

```

  

Requirement already satisfied: SQLAlchemy in /home/damar.pramuditya/.pyenv/versions/anaconda3-2022.05/lib/python3.9/site-packages (1.4.32)

Requirement already satisfied: greenlet!=0.4.17 in /home/damar.pramuditya/.pyenv/versions/anaconda3-2022.05/lib/python3.9/site-packages (from SQLAlchemy) (1.1.1)

  
  
  

```python

from sqlalchemy import create_engine

  

# Connect to the database

engine = create_engine('mysql+pymysql://root:root@localhost:3307/logs')

  

try:

with engine.begin() as connection:

logs_df.to_sql(name='logs_df', con=engine, if_exists='replace', index=False)

print('Sucessfully written to Database!!!')

except Exception as e:

print(e)

```

  

Sucessfully written to Database!!!

  
  

  

#### Setup MySQL

  

  

Using docker to setup a database is chosen because it is a handy and effortless way to set up a database. Run this command in your terminal 

  

  

docker run -p 3307:3306 --hostname localhost --name data-science-asg -e MYSQL_ROOT_PASSWORD=root -d mysql

  

  

For load into database MySQL using library SQLAlchemy. SQLAlchemy is the Python SQL toolkit and Object Relational Mapper that gives application developers the full power and flexibility of SQL. It provides a full suite of well known enterprise-level persistence patterns, designed for efficient and high-performing database access, adapted into a simple and Pythonic domain language.

  

  

from sqlalchemy import create_engine

# Connect to the database
```
engine = create_engine('mysql+pymysql://root:root@localhost:3307/logs')

try:

    with engine.begin() as connection:

        logs_df.to_sql(name='logs_df', con=engine, if_exists='replace', index=False)

        print('Sucessfully written to Database!!!')

except Exception as e:

    print(e)

```



  

## Visualize Data

  

As the final process transform data from unstructured data to structured data, In this study will provide frontend tools to serve the data, we choose open source tools:

  

### Setup Metabase

  

We choose docker as well for setting up these tools. 

  

  

Use this quick start to run the Open Source version of Metabase locally. See below for instructions on [running Metabase in production](https://www.metabase.com/docs/latest/installation-and-operation/running-metabase-on-docker#production-installation).Assuming you have [Docker](https://www.docker.com/) installed and running, get the latest Docker image:

  
```
docker pull metabase/metabase:latest

  

  

Then start the Metabase container:

  

  

docker run -d -p 12345:3000 --name metabase metabase/metabase
```



### Connect to your data source 

  

# ![](https://lh3.googleusercontent.com/yE5vEodaXchBzw60D6VPAdAqPYIMftWPuUywoKVeIe1RJYZMCvwOZodiemdMOIMznZ2EoDrR8j3a1AjDbu_U0N9jx6T6HkRrdOVBZGAFItDElJTzO6AW31AKRAt4NjTpKCrlIowfs4lo1B6Q-FEUer8xeOSN-R_4sDou6poOjqpFhIu3Fu-Uclc2zA)

  

Save and your dashboard should be ready.

  

![](https://lh6.googleusercontent.com/ROpbKLRM6rC688jAujXjiT-r_fAXXwjUloaUM_CdwvqiWVqpI6u2-rwfANEmy4rbPgNpEOOu106y6exQuQq561uQ-Ep09S4frMM1-9o3BlVVdKh1-Wl5VAKJqXZEYtQXODjUQ5s8sEULqiLxi8rxG00BEElPbeCn-Wp0V6ckRfrMlk3M_RMqx4PCcg)

  