# INSTATE

INSTATE (Multidimensional indexing for reliable and scalable IoT data management) aims to
**enable a novel reliable and scalable architecture for data management for IoT applications**,
using multidimensional indexing to support efficient query, searching, and analytics over data.

## OVERVIEW

The code of INSTATE adds the necessary automatization to Qbeast Spark code to be built on top of AWS or other Cloud Provider's Architecture for Streaming IoT data into an Object Storage (in this case, S3), applying Qbeast Layout to organize it efficiently. 

An image of the central pieces of the architecture.
![image](https://github.com/Qbeast-io/INSTATE/assets/22685017/e7e8cea2-b58c-496e-90ef-181e2c103d35)



### Components

1. Streaming Source. The source can be any type of IoT device that it's continously generating data, such as: image, device activity, geolocalization...
2. Spark Streaming App. Set up and configure a Spark Streaming application that reads from the generated data and writes using an optimized Qbeast layout.
3. Qbeast Layout. Organization of S3 files for faster and more resource-efficient retrieval. (Find all the specifications for the format at https://github.com/Qbeast-io/qbeast-spark)


## QUICKSTART

The core of INSTATE is Qbeast Format: a layout format that organizes the information in files using indexing and sampling techniques. 

To get started with Qbeast Format, you can use the first reference Open Source implementation for Apache Spark.

#### 1. Install Apache Spark

```shell
wget https://archive.apache.org/dist/spark/spark-3.4.2/spark-3.4.2-bin-hadoop3.tgz

tar -xzvf spark-3.4.2-bin-hadoop3.tgz

export SPARK_HOME=$PWD/spark-3.4.2-bin-hadoop3
```

#### 2. Start Spark Shell

```shell
$SPARK_HOME/bin/spark-shell \
--packages io.qbeast:qbeast-spark_2.12:0.5.0,io.delta:delta-core_2.12:2.1.0 \
--conf spark.sql.extensions=io.qbeast.spark.internal.QbeastSparkSessionExtension \
--conf spark.sql.catalog.spark_catalog=io.qbeast.spark.internal.sources.catalog.QbeastCatalog
```

#### 3. Write data with Qbeast

```scala
val data = Seq((1, "a", 10), (2, "b", 20), (3, "c", 30)).toDF("id", "name", "age")
data.write.format("qbeast").option("columnsToIndex", "id,age").save("/tmp/qbeast_test")
```

#### 4. Query the data


```scala

val indexed_data = spark.read.format("qbeast").load("/tmp/qbeast_test")
indexed_data.filter("id > 2 and age > 20").show()
```

## EXAMPLES

In the [notebooks folder](notebooks), you will find examples of use for IoT public datasets. 
