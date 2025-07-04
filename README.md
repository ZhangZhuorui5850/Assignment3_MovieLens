# MovieLens Assignment 3 - PySpark + Cassandra + Zeppelin

This repository presents Assignment 3 for the Data Management course. The task is to analyze the MovieLens 100k dataset using **PySpark**, store the data in **Apache Cassandra**, and run SQL-based queries within a **Zeppelin notebook**.

---

## 📂 Directory Structure

```
Assignment3_MovieLens/
│
├── data/                            
│   ├── u.data
│   ├── u.genre
│   ├── u.item
│   └── u.user
│
├── docs/                             ← Documentation (analysis + setup)
│   ├── INSTALL.md                    Zeppelin + Cassandra setup guide
│   └── REPORT.md                     SQL query explanations
│
├── notebooks/                        ← Zeppelin notebook and assets
│   ├── Assignment3.json              Zeppelin notebook export (JSON)
│   ├── Zeppelin.png                  Screenshot of the Zeppelin
│   └── Zeppelin.pdf                  Exported Zeppelin as PDF
│
├── screenshots/                      ← Query results 
│   ├── 1-movie-average-ratings.png
│   ├── 2-top-10-highest-rated-movies.png
│   ├── 3-user-favorite-genre-by-ratings.png
│   ├── 4-users-under-20.png
│   └── 5-scientists-aged-30-40.png
│             
└── README.md                       
```

<p align="center">
  <a href="notebook/Zeppelin.png" target="_blank">
    📄 <strong>Open the Notebook</strong>
  </a>
</p>



---

## 🚀 How to Use

1. **Environment:** Apache Zeppelin + PySpark + Cassandra
2. **Import Notebook:** Upload `Assignment3.json` into Zeppelin
3. **Upload Dataset:** Place MovieLens `u.user`, `u.data`, `u.item`, `u.genre` into HDFS or local path
4. **Run Notebook:** Execute all code cells step-by-step
5. **Match Outputs:** Refer to screenshots in `/results/` for reference

🛠 [Zeppelin + Cassandra Setup Guide](docs/INSTALL.md)
---

## 📊 Implemented SQL Queries

### (i) Average Rating Per Movie
- **Description:** Computes average rating grouped by movie ID
- **Enhancement:** Joined with `movies` table to display `title`
- **Screenshot:**  
![](screenshots/1-movie-average-ratings.png)

### (ii) Top 10 Highest-Rated Movies
- **Description:** Shows 10 movies with highest average ratings
- **Enhancement:** Includes both `title` and `movie_id`
- **Screenshot:**    
![](screenshots/2-top-10-highest-rated-movies.png)

### (iii) Active Users' Favorite Genre
- **Description:** For users with ≥50 ratings, find their most rated genre
- **Enhancement:** Added `gender` and `occupation` from `users` table
- **Screenshot:**  
![](screenshots/3-user-favorite-genre-by-ratings.png)

### (iv) Users Under 20
- **Description:** Shows user info where `age < 20`
- **Enhancement:** Sorted by age
- **Screenshot:**  
![](screenshots/4-users-under-20.png)

### (v) Scientists Aged 30–40
- **Description:** Filter `occupation = 'scientist'` and age between 30–40
- **Screenshot:**  
![](screenshots/5-scientists-aged-30-40.png)

---

##  Notes

- Zeppelin Markdown and visual output included
- Cassandra keyspace used: `movielens`
- All three tables (`users`, `ratings`, `movies`) persisted in Cassandra

📄 [View Full Report](docs/REPORT.md)  
---
## 🧩 Challenges & Solutions

### 1. Jupyter Notebook: Unable to Connect to Cassandra

**Problem:**  
Code that worked in Zeppelin failed in Jupyter Notebook with errors such as NoClassDefFoundError or Cassandra connection refused.

**Cause:**  
Jupyter Notebook does not automatically load required JAR dependencies. In addition, Spark cannot reach Cassandra unless configured explicitly.

**Solution:**  
- This problem has not been solved yet

---

### 2. Zeppelin: Cassandra Connector Not Recognized

**Problem:**  
In Zeppelin, spark or pyspark interpreter failed with errors like "Class not found" for Cassandra-related classes.

**Cause:**  
Zeppelin runs under a restricted system user and cannot access `.jar` files placed in the user's home directory.

**Solution:**  
- Downloaded required connector `.jar` files:
  - `spark-cassandra-connector_2.11-2.3.0.jar`
  - `cassandra-driver-core-3.5.0.jar`
- Moved them to Zeppelin’s global JAR directory:

```bash
sudo mkdir -p /usr/lib/zeppelin/jars
sudo cp *.jar /usr/lib/zeppelin/jars/
```

- Configured the Spark interpreter under Zeppelin:

```text
spark.jars = /usr/lib/zeppelin/jars/spark-cassandra-connector_2.11-2.3.0.jar,/usr/lib/zeppelin/jars/cassandra-driver-core-3.5.0.jar
spark.cassandra.connection.host = 127.0.0.1
```

- Restarted the interpreter and re-ran the notebook

---
