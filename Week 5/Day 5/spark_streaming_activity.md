# Structured Streaming Activity


## Objective

Practice working with structured streaming in Databricks using Notebooks.

## Instructions

### Locate Sample Data

```
%fs ls /databricks-datasets/structured-streaming/events/
```

This sample data includes a directory of many JSON files. Each line in the JSON record contains two fields: *time* and *action*.

### Initialize the stream

```python
from pyspark.sql.types import StructType, StructField, TimestampType, StringType
from pyspark.sql.functions import col, window

inputPath = "/databricks-datasets/structured-streaming/events/"

# Define the schema to speed up processing
jsonSchema = StructType([ StructField("time", TimestampType(), True), StructField("action", StringType(), True) ])

streamingInputDF = (
  spark
    .readStream
    .schema(jsonSchema)               # Set the schema of the JSON data
    .option("maxFilesPerTrigger", 1)  # Treat a sequence of files as a stream by picking one file at a time
    .json(inputPath)
)

streamingCountsDF = (
  streamingInputDF
    .groupBy(
      streamingInputDF.action,
      window(streamingInputDF.time, "1 hour"))
    .count()
)
```



### Start the Streaming Job

```python
query = (
  streamingCountsDF
    .writeStream
    .format("memory")        # memory = store in-memory table (for testing only)
    .queryName("counts")     # counts = name of the in-memory table
    .outputMode("complete")  # complete = all the counts should be in the table
    .start()
)

```

*query* is a handle to the streaming query named *counts* that is running in the background. This query continuously picks up files and updates the windowed counts. The command window reports the status of the stream.

### Interactively Query the Stream

```sql
%sql select action, date_format(window.end, "MMM-dd HH:mm") as time, count from counts order by time, action
```

