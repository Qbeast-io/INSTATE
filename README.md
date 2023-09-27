# INSTATE

INSTATE (Multidimensional indexing for reliable and scalable IoT data management) aims to
**enable a novel reliable and scalable architecture for data management for IoT applications**,
using multidimensional indexing to support efficient query, searching, and analytics over data


## Quickstart

The core of INSTATE is Qbeast Format: a layout format that organizes the information in files using indexing and sampling techniques. 

To get started with Qbeast Format, you can use the first reference Open Source implementation for Apache Spark.

#### 1. Install Apache Spark

```shell
wget https://archive.apache.org/dist/spark/spark-3.3.0/spark-3.3.0-bin-hadoop3.tgz

tar -xzvf spark-3.3.0-bin-hadoop3.tgz

export SPARK_HOME=$PWD/spark-3.3.0-bin-hadoop3
```

#### 2. Start Spark Shell

```shell
$SPARK_HOME/bin/spark-shell \
--packages io.qbeast:qbeast-spark_2.12:0.4.0,io.delta:delta-core_2.12:2.1.0 \
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

## Notebooks

In the [notebooks folder](notebooks), you will find examples of use for IoT public datasets. // TODO