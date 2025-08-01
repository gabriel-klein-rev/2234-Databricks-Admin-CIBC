# Activity: Working with Spark DataFrames using Databricks Notebooks

## Objective
Learn how to read, explore, and manipulate a Spark DataFrame using the `data_geo.csv` sample dataset from `/databricks-datasets/population-vs-price/`. This activity will explore loading data, inspecting schemas, filtering, selecting columns, and writing data back to DBFS.

---

## Instructions

1. **Create a new notebook** in your workspace and attach it to your cluster.

2. **List the sample dataset directory** to confirm the file exists:
   ```python
   %fs ls /databricks-datasets/samples/population-vs-price/
   ```

3. **Load the CSV file as a DataFrame**:
   ```python
   df = spark.read.option("header", "true").csv("/databricks-datasets/samples/population-vs-price/data_geo.csv")
   ```

4. **Inspect the schema and preview the data**:
   ```python
   df.printSchema()
   df.show(5)
   ```

5. **Count the number of rows in the dataset**:
   ```python
   row_count = df.count()
   print(f"Row count: {row_count}")
   ```

6. **Select only the columns `State`, `2014 Population estimate`, and `2015 median sales price`**:
   ```python
   selected_df = df.select("State", "2014 Population estimate", "2015 median sales price")
   selected_df.show(10)
   ```

7. **Filter the dataset to show only states with home prices above 400,000 in 2015**:
   ```python
   filtered_df = selected_df.filter(selected_df["2015 median sales price"] > 400)
   filtered_df.show()
   ```

   If you get an error, try casting the columns:
   ```python
   from pyspark.sql.functions import col
   filtered_df = selected_df \
      .withColumn("2015 median sales price", col("2015 median sales price") \
      .cast("double")) \
      .filter(col("2015 median sales price") > 400)

   filtered_df.show()
   ```

8. **Write the filtered data back to DBFS as a Parquet file**:
   ```python
   filtered_df.write.mode("overwrite").parquet("/tmp/high_price_states")
   ```

9. **List the output location** to confirm the write was successful:
   ```python
   %fs ls /tmp/high_price_states
   ```

---

## Summary / Review Questions

1. What does `printSchema()` show you about a DataFrame?
2. Why do we sometimes need to cast columns before filtering?
3. What are the benefits of writing data in Parquet format?
4. How would you modify the filter to include states with prices between $300,000 and $500,000?
5. What is the difference between `.select()` and `.filter()` in PySpark?

---

## Additional practice

- Create a column that shows the cities ranked by 2015 median sales price.
- Create a dataframe that compares each cities' population rank and 2015 median sales price rank
