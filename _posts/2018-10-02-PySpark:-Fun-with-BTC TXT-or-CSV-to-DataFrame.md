---
published: true
layout: post
category: PySpark
---

#PySpark provides users with the choice to convert [RDD's(Resilient Distributed Dataset)](https://spark.apache.org/docs/0.6.1/api/core/spark/RDD.html) to DataFrame's and manipulate data in a manner similar to SQL. Below is quick exercise highlighting the ease of the conversion.

```Py
	from pyspark.sql.types import *
	from pyspark import SQLContext, Rows
	from pyspark import SparkConf, SparkContext
```

#Set Configuration and load SparkContext and SQLContext  

```Py
	conf = SparkConf().setMaster("local").setAppName("BTC")

	sc = SparkContext.getOrCreate(conf = conf)

	sqlContext = SQLContext(sc)
```

#Load data from existing .txt file.
##Data Being Used is the BTC Global Price Index data available via [quandl](https://www.quandl.com/search?query=)
#for .txt files

```Py 
	data_file = '/home/riverstone/Downloads/NW_BTC_Global_Price_INDX'
```
#for .csv files

```Py 
	data_file = '/home/riverstone/Downloads/NW_BTC_Global_Price_INDX.csv'
```
#Load data into an RDD and format it using the .split(",") 

```Py
	BTC_raw = sc.textFile(data_file).map(lambda b: b.split(","))
```
#Name the column rows to be applied to the DataFrame. Essential, otherwise the dataframe with have column names such as "_1","_2", etc.

```Py	
	row_data =BTC_raw.map(lambda p: Row(Date=p[0],Open=p[1],High=p[2],Low=p[3],Close=p[4],Volume=p[5],VWAP=p[6],TWAP=p[7]))
```

#Create the DataFrame and Check the data and schema for good measure

```Py
	BTC_df = BTC_raw.toDF()
	BTC_df.show()
	BTC_df.printSchema()
	root
 		|-- Close: string (nullable = true)
 		|-- Date: string (nullable = true)
 		|-- High: string (nullable = true)
 		|-- Low: string (nullable = true)
 		|-- Open: string (nullable = true)
 		|-- TWAP: string (nullable = true)
 		|-- VWAP: string (nullable = true)
 		|-- Volume: string (nullable = true)
```

#Explore a brief summary of your desired column

```Py
	BTC_df.describe("Volume").show()
	+-------+-----------------+
	|summary|           Volume|
	+-------+-----------------+
	|  count|             1416|
	|   mean|652234.4298469605|
	| stddev|952240.0493998282|
	|    min| 1002122.88827962|
	|    max|           Volume|
	+-------+-----------------+
```
#Select Multiple Columns

```Py
	BTC_df.select("Date","VWAP","Volume").show(5)
	+----------+------------+---------------+
	|      Date|        VWAP|         Volume|
	+----------+------------+---------------+
	|      Date|        VWAP|         Volume|
	|2014-04-01|482.75743985| 74776.47884546|
	|2014-04-02|   460.19242|114052.96112562|
	|2014-04-03|432.28588464| 91415.08017749|
	|2014-04-04|443.45808586| 51147.27201926|
	+----------+------------+---------------+
	only showing top 5 rows
```
#Create new columns with the .withColumn('New Column',data for column). Here I have created a column named '%DifVol' calculated by subtracting and dividing by the mean volume(meanVol). The meanVol was hard coded from the data provided by .describe above.

```Py
	BTC_df.withColumn('%DifVol',(BTC_df.Volume - meanVol)/meanVol).withColumn('Range',BTC_df.High-BTC_df.Low).select('Date','Range','%DifVol').show(5)
```
##Using traditional SQL methods of extracting and manipulating data can be used as well.

```Py
	BTC_df.registerTempTable("coins")
```
#Below the Date and Closing prices of when BTC was above the [VWAP(Volume Weighted Average Price)](https://en.wikipedia.org/wiki/Volume-weighted_average_price), above the Open, and the difference between the High minus Low divided by the Low was greater than 25%.

```Py
	sqlContext.sql('''SELECT Date, Close FROM coins WHERE Close > VWAP AND Close > Open AND (High-Low)/Low> 0.25 ''').show()
	+----------+--------------+
	|      Date|         Close|
	+----------+--------------+
	|2015-01-15|  208.89408044|
	|2017-07-20| 2812.84956552|
	|2017-09-15| 3630.04923277|
	|2017-12-07|18637.87899415|
	|2018-02-06| 7757.30430767|
	+----------+--------------+
```
