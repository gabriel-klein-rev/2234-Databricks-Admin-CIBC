# Activity: Processing Squirrel Data with Databricks

## Objective

- Get more practice with creating dataframes, processing data, and saving to a volume. 

## Instructions

    - Load squirrel census data (located in the squirrel_census schema, park-data.csv, squirrel-data.csv) into Pyspark Dataframes
    - Join the two dataframes to denormalize the data
    - Create a dataframe with just the Squirrel ID, Park Name, Primary Color, Activities, and Park Conditions
    - Save that dataframe to a table in the squirrel_census schema titled {name}-squirrel-data

## When complete

    - Explore the data with Databricks SQL
        - Where are most of the squirrel sightings?
        - What activities are most common at each park?
        - Any other queries you are curious about

    