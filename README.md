# Assignment 3 – MovieLens Data Management Pipeline

## Overview

This repository contains the solution to Assignment 3 of the Data Management course.  
I build a distributed ETL and analytics pipeline using Spark + Cassandra on the MovieLens 100k dataset, fully implemented in Apache Zeppelin.

## Features

- Load MovieLens data from HDFS using PySpark
- Transform and clean user, movie, and rating data
- Store data into Cassandra with appropriate schema
- Perform analytical queries using Spark SQL
- Visualize results with Zeppelin and Markdown
- Organize content in modular, reproducible Notebook format

## Folder Structure

| Folder        | Content                                       |
|---------------|-----------------------------------------------|
| `notebook/`   | Zeppelin notebook JSON (`Assignment3_MovieLens.json`) |
| `data/`       | Raw data files (user, item, data, genre)      |
| `screenshots/`| Step-by-step output snapshots (Spark + SQL)   |
| `docs/`       | (Optional) PDF/Markdown summary and extra analysis |

## Learning Highlights

- Applied ETL in Spark: from HDFS → DataFrame → Cassandra
- Designed data models for NoSQL (composite keys, sets)
- Used Cassandra + Spark integration via connector
- Wrote SQL-like analytics to answer 5 tasks from assignment
- Demonstrated reproducibility and documentation in Zeppelin

## Future Extensions

- Integrate Apache Airflow or Oozie for automation
- Add streaming ingest and real-time dashboard via Superset
- Extend to MovieLens 1M or Netflix dataset for scalability test

## Submitted for

- Course: Data Management
- Student: [Zhang Zhuorui]
- Date: 16/June/2025
