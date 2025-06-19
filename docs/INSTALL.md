# ⚙️ Zeppelin + Cassandra Integration Guide

This document explains how to configure **Apache Zeppelin** to connect to **Apache Cassandra** using **Spark-Cassandra Connector** for Assignment 3.

---

## 📦 1. Required JAR Dependencies

Download the following JAR files:

- [spark-cassandra-connector_2.11-2.3.0.jar](https://repo1.maven.org/maven2/com/datastax/spark/spark-cassandra-connector_2.11/2.3.0/spark-cassandra-connector_2.11-2.3.0.jar)
- [cassandra-driver-core-3.5.0.jar](https://repo1.maven.org/maven2/com/datastax/cassandra/cassandra-driver-core/3.5.0/cassandra-driver-core-3.5.0.jar)

Move them to Zeppelin’s JAR directory:

```bash
sudo mkdir -p /usr/lib/zeppelin/jars
sudo cp /home/maria_dev/Assignment3/*.jar /usr/lib/zeppelin/jars/
```

⚠️ **Why this path?**  
Zeppelin runs under a separate `zeppelin` user. It must be able to access the connector JARs placed inside its default path (`/usr/lib/zeppelin/jars/`).

---

## 🛠 2. Configure Spark Interpreter in Zeppelin

Steps:

1. Go to ⚙️ → *Interpreter* → Select `spark`
2. Add the following properties:

| Property | Value |
|----------|-------|
| `spark.jars` | `/usr/lib/zeppelin/jars/spark-cassandra-connector_2.11-2.3.0.jar,/usr/lib/zeppelin/jars/cassandra-driver-core-3.5.0.jar` |
| `spark.cassandra.connection.host` | `127.0.0.1` |
| `spark.cassandra.connection.port` | `9042` |

Then click **Save** → **Restart Interpreter**

---

## ✅ 3. Verify Connection (Optional Test)

In `cqlsh`, create a test keyspace and table:

```sql
CREATE KEYSPACE IF NOT EXISTS testks 
WITH REPLICATION = {'class': 'SimpleStrategy', 'replication_factor': 1};

USE testks;

CREATE TABLE IF NOT EXISTS students (
    id int PRIMARY KEY,
    name text,
    age int
);

INSERT INTO students (id, name, age) VALUES (1, 'Alice', 20);
INSERT INTO students (id, name, age) VALUES (2, 'Bob', 22);
INSERT INTO students (id, name, age) VALUES (3, 'Charlie', 19);
```

In Zeppelin, test reading from Cassandra:

```scala
import com.datastax.spark.connector._

// Load Cassandra table as RDD
val rdd = sc.cassandraTable("testks", "students")

// Preview records
rdd.take(5).foreach(println)
```

---

✅ If this prints the inserted records, your Spark ↔ Cassandra integration is working!
