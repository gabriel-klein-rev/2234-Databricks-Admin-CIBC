# Week 5 Day 2 (7/22/2025)

## Jobs

In Databricks, a job is used to schedule and orchestrate tasks on Databricks in a workflow. Common data processing workflows include ETL workflows, running notebooks, and machine learning (ML) workflows, as well as integrating with external systems like dbt.

A job is the primary resource for coordinating, scheduling, and running your operations. Jobs can vary in complexity from a single task running a Databricks notebook to hundreds of tasks with conditional logic and dependencies. The tasks in a job are visually represented by a Directed Acyclic Graph (DAG). You can specify properties for the job, including:

1. Trigger - this defines when to run the job.
2. Parameters - run-time parameters that are automatically pushed to tasks within the job.
3. Notifications - emails or webhooks to be sent when a job fails or takes too long.
4. Git - source control settings for the job tasks.

## Task 

A task is a specific unit of work within a job. Each task can perform a variety of operations, including:

1. A notebook task runs a Databricks notebook. You specify the path to the notebook and any parameters that it requires.
2. A pipeline task runs a pipeline. You can specify existing Lakeflow Declarative Pipelines, such as a materialized view or streaming table.
3. A Python script tasks runs a Python file. You provide the path to the file and any necessary parameters.

There are many types of tasks. Tasks can have dependencies on other tasks, and conditionally run other tasks, allowing you to create complex workflows with conditional logic and dependencies.

## Trigger

A trigger is a mechanism that initiates running a job based on specific conditions or events. A trigger can be time-based, such as running a job at a scheduled time (for example, ever day at 2 AM), or event-based, such as running a job when new data arrives in cloud storage.

## Pipelines

Lakeflow Declarative Pipelines is a framework for creating batch and streaming data pipelines in SQL and Python. Common use cases for Lakeflow Declarative Pipelines include data ingestion from sources such as cloud storage (such as Amazon S3, Azure ADLS Gen2, and Google Cloud Storage) and message buses (such as Apache Kafka, Amazon Kinesis, Google Pub/Sub, Azure EventHub, and Apache Pulsar), and incremental batch and streaming transformations.