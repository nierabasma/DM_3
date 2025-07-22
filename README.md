# 🎬 MovieLens 100k Data Analysis with Spark & Cassandra

This project leverages **Apache Zeppelin**, **Spark SQL**, and **Cassandra** to analyze the [MovieLens 100k dataset](https://grouplens.org/datasets/movielens/100k/). The focus is on exploring user behavior, movie preferences, and genre distribution through distributed data processing.

![Movies](https://upload.wikimedia.org/wikipedia/commons/2/20/Movie_Projector_Icon.png)

---

## 📌 Project Overview

The MovieLens 100k dataset includes **100,000 ratings** from **943 users** on **1682 movies**. Using Spark and Cassandra integration, the notebook performs the following tasks:

1. **Load data into Cassandra** using PySpark from HDFS.
2. **Calculate the average rating for each movie**.
3. **Identify the top 10 movies with the highest average ratings**.
4. **Determine favorite genres for users who rated ≥ 50 movies**.
5. **Filter users under age 20**.
6. **Find users aged 30–40 with occupation “scientist”**.

This project demonstrates practical usage of Spark for large-scale data manipulation and NoSQL storage with Cassandra.

---

## 🧰 Tools & Technologies

![Spark](https://img.shields.io/badge/Apache%20Spark-FF5F00?style=flat&logo=apache-spark&logoColor=white)
![Zeppelin](https://img.shields.io/badge/Apache%20Zeppelin-2c3e50?style=flat&logo=apache&logoColor=white)
![Cassandra](https://img.shields.io/badge/Apache%20Cassandra-1287B1?style=flat&logo=apache-cassandra&logoColor=white)
![Python](https://img.shields.io/badge/PySpark-3670A0?style=flat&logo=python&logoColor=white)

- **Apache Zeppelin**: Interactive analysis interface
- **Apache Spark (PySpark)**: Distributed data processing
- **Apache Cassandra**: NoSQL data storage
- **HDFS**: Storage for raw data input

---

## 📂 Dataset Files

- `/tmp/u.user`: User demographics (user_id, age, gender, occupation, zip code)  
- `/tmp/u.data`: Ratings (user_id, movie_id, rating, timestamp)  
- `/tmp/u.item`: Movie metadata and genres (movie_id, title, genre flags)

---

## 🔍 Analysis Tasks

### 1. Load Data from HDFS and Save to Cassandra
- Transformed raw `.user`, `.data`, and `.item` files into structured PySpark DataFrames.
- Wrote them to Cassandra tables: `users`, `rate`, and `topic_name`.

---

### 2. Average Rating per Movie
Used Spark SQL to join rating and movie tables and compute average ratings for each movie.

```python
avg_ratings_df = joined_df.groupBy("title").agg(avg("rating").alias("avg_rating"))
```

---

### 3. Top 10 Highest-Rated Movies

Identified top 10 titles based on average rating:

```text
+----------------------------+------------+
| Title                      | Avg Rating |
+----------------------------+------------+
| Aiqing wansui (1994)       | 5.0        |
| Marlene Dietrich: Her Own | 5.0        |
| Someone Else's America     | 5.0        |
| Star Kid (1997)            | 5.0        |
| Prefontaine (1997)         | 5.0        |
+----------------------------+------------+
```

---

### 4. Favorite Genre for Users Who Rated ≥ 50 Movies

Used genre flags and exploded genre preferences. Top users by genre:

```text
+--------+--------------+-------+
| userID | Favorite     | Count |
+--------+--------------+-------+
| 655    | genre_drama  | 410   |
| 405    | genre_drama  | 309   |
| 279    | genre_comedy | 211   |
```

---

### 5. Users Under 20 Years Old

Filtered `users` table:

```text
user_id | age | occupation
--------|-----|------------
482     | 18  | student
303     | 19  | student
101     | 15  | student
```

---

### 6. Scientists Aged Between 30–40

Used Spark filter condition:

```python
(col("age") > 30) & (col("age") < 40) & (col("occupation") == "scientist")
```

Sample Output:

```text
user_id | age | occupation
--------|-----|------------
272     | 33  | scientist
430     | 38  | scientist
643     | 39  | scientist
```

---

## ⚙️ Setup Instructions

### Prerequisites
- Apache Hadoop & HDFS
- Apache Spark (configured with Zeppelin)
- Apache Zeppelin
- Apache Cassandra

---

### ⚡ How to Run

1. **Download Dataset via Shell** (built into Zeppelin):
   ```bash
   wget http://media.sundog-soft.com/hadoop/ml-100k/u.user -O /tmp/u.user
   wget http://media.sundog-soft.com/hadoop/ml-100k/u.data -O /tmp/u.data
   wget http://media.sundog-soft.com/hadoop/ml-100k/u.item -O /tmp/u.item
   ```

2. **Start Zeppelin Notebook**
   - Open `STQD6324_ASSIGNMENT3_P145327.json` in Apache Zeppelin
   - Run cells sequentially

3. **Check Cassandra**
   - Ensure `movielens` keyspace and required tables (`users`, `rate`, `topic_name`) exist

---

## 📌 Notes

- Spark version used: **2.3.0.2.6.5.0-292**
- Zeppelin notebook ID: `STQD6324_ASSIGNMENT3_P145327`
- Cassandra used as a distributed, column-oriented NoSQL store for scalability
