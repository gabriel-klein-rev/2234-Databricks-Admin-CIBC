# Spark Review

## What does Cluster Computing refer to?

Cluster computing refers to the process of sharing computation tasks among multiple machines which works together to act as a single machine

## What does RDD stand for?

Resilient Distributed dataset

## What key APIs does Spark provide?

RDD, DataFrame, Dataset (not available in Python)

## What languages does Spark provide APIs for?

Java, Scala, Python, R

## What does it mean when we say an RDD is a collection of objects partitioned across a set of machines?

An RDD is distributed, meaning the data is stored across multiple nodes. This helps the data to be resilient. Now, the data itself that is contained in the RDD is not copied across to create resilience, but the lineage of the RDD is, so that the data that is accessed after an action can still be accessed.

## SparkSession vs SparkContext?

SparkContext
    - 	Spark SparkContext is an entry point to Spark and defined in org.apache.spark package since 1.x and used to programmatically create Spark RDD, accumulators and broadcast variables on the cluster. Since Spark 2.0 most of the functionalities (methods) available in SparkContext are also available in SparkSession. Its object sc is default available in spark-shell and it can be programmatically created using SparkContext class.

SparkSession
    -	SparkSession introduced in version 2.0 and and is an entry point to underlying Spark functionality in order to programmatically create Spark RDD, DataFrame and DataSet. It’s object ‘spark’ is default available in spark-shell and it can be created programmatically using SparkSession builder pattern.


## What is the lineage of an RDD?

The list of transformations that the RDD has gone through.

## RDDs are lazy and ephemeral. What does this mean?

The processing of transformations do not occur until an action is called, and the dataset is discarded from memory after use

## What are the 4 ways provided to construct an RDD?

Parallelize, from another RDD, from DF/DS, from file

## What does it mean to cache an RDD? *

Caching is an optimization tactic. It saves the data from an RDD action instead of deleting that data right away so that the RDD can be used again on a later transformation or action. This can be helpful if many different transformations and actions are called from a common RDD.

## What does it mean to perform a parallel operation on an RDD?

Since the RDD is partitioned, operations can be done in parallel, where multiple executors can perform the same action across those partitions

## Why does Spark need special tools for shared variables, instead of just declaring, for instance, var counter=0? * 

Because operations happen in parallel across multiple partitions, shared variables need to be able to be accessed in each of those places in memory

## What is a broadcast variable? * 

A broadcast variable is a read-only variable that is shared on all nodes. They are immutable and distributed. 

## What is a Spark Application? Job? Stage? Task?

-	Application- main function 
-	Jobs- Work submitted to Spark. Created when an action occurs
-	Stage- Jobs are divided into stages
-	Task- Each stage is divided into tasks, the smallest unit of work for Spark

## Different ways to repartition?

-	.repartition
-	.coalesce (to reduce partitions)


## What is the logical plan?  The physical plan?

-	The logical plan is the tree that represents both the data and schema. They depict the expected output
-	The physical plan decides what type of join, sequence of execution of many methods


## How many partitions does a single task work on?

One

## What is a Dataframe?

A DataFrame is a distributed collection of data organized into named columns, similar to a table in a relational database. It provides an abstraction for working with structured and semi-structured data in Spark, enabling efficient data processing and querying across large datasets. It is a higher-level abstraction compared to RDD's and offers a more optimized and user-friendly API for working with structured data. 

## How would we go about using Spark SQL to query Dataframes?

- Initialize a SparkSession
- Create or load a DataFrame
- Register the df as a temporary view
- Run queries using spark.sql()
