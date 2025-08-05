# Census Data Project (8/21/2025)

The associates will work on the United States Census data from the last three decades, 2000, 2010 and 2020. This data is frequently used by organizations to determine the amount of social aid or need a community might need. It is also used to determine redistricting information for the United States. 

## Data
2000 - https://www.census.gov/data/datasets/2000/dec/redistricting.html

2010 - https://www.census.gov/data/datasets/2010/dec/redistricting-file-pl-94-171.html

2020 - https://www.census.gov/programs-surveys/decennial-census/about/rdo/summary-files.2020.html

## Project Setup

The project setup is vital to the success of the project. Ensure that these steps are completed before any development takes place. Utilizing the steps below to prepare for development can increase the probability of success and decrease stress levels. Here are the steps to follow:

1) Set up a GitHub Repository for the project
2) Set up an org for this project
3) Clone to your computer and initialize

## Phase 1

To begin this project, the associates must work together to extract value from the Census data. The raw data itself will be already ingested into a bronze layer schema on the Databricks Workspace. Each decade's population data is separated by state. Once extracted the data must be cleaned to glean the important fields needed for analysis and stored in a public cloud storage so every team member has access to the data. The associates should have a working data pipeline through Databricks/Pyspark that handles the core functionality listed below:

1) Ingest Bronze layer data
2) Using Databricks/Pyspark, clean data of irrelevant fields or unusable data to create a Silver layer
3) Aggregate data into analysis-ready Gold layer tables
4) Run SQL queries to get an initial summary of data

## Phase 2

Phase 2’s purpose is to enforce data governance practices, manage users and groups, setting table-level permissions, and ensuring a tight medallion architecture. Trainees will be asked to provide reasoning for their practices and implementation.

#### Analysis

- What is the total population of all the US states/territories?
- Which region has the highest population in each year?
- Which states are growing the fastest? (total increase, percent increase)
- Which regions are growing the fastest? (total population increase, percent increase)
- Compare the population increase from 2000 to 2020 and predict the population for 2030. (Include some states of interest as well)
- Any other trends that you may find with the population data that could predict the future.

## Presentation

At the end of this project, you and your team will be required to give a professional presentation. In this presentation, you should cover the technologies utilized, architecture diagram, list of features implemented, queries answered, outstanding defects/issues, challenges faced, and give a thorough project demo.