#  Assignment 3 Report: Query Design and Explanation

This document explains the logic and implementation behind the five SQL queries executed in Apache Zeppelin.

---

# Query (i): Average Rating Per Movie

### Objective:
- Compute the average rating received by each movie.

### Method:
- Use `ratings` table: `GROUP BY movie_id`, calculate `AVG(rating)`
- Join with `movies` to show `title`

```sql
SELECT r.movie_id, m.title, ROUND(AVG(r.rating), 2) AS avg_rating
FROM ratings r
JOIN movies m ON r.movie_id = m.movie_id
GROUP BY r.movie_id, m.title
ORDER BY r.movie_id
```

---

## Query (ii): Top 10 Highest-Rated Movies

### Objective:
- Show top 10 movies with the highest average ratings

### Method:
- Same as Query (i) but `ORDER BY avg_rating DESC`
- Added `LIMIT 10` and retained `movie_id` + `title`

```sql
SELECT r.movie_id, m.title, ROUND(AVG(r.rating), 2) AS avg_rating
FROM ratings r
JOIN movies m ON r.movie_id = m.movie_id
GROUP BY r.movie_id, m.title
ORDER BY avg_rating DESC
LIMIT 10
```

---

## Query (iii): Active Users' Favorite Genre

### Objective:
- For users with ≥ 50 ratings, find the genre they rated the most

### Method:
1. `WITH active_users` filters users with ≥50 ratings  
2. `JOIN ratings + movies` and `explode(genres)`  
3. `COUNT(*)` by (user_id, genre)  
4. Use `ROW_NUMBER()` to rank genre counts  
5. Join with `users` to fetch `gender` and `occupation`

```sql
WITH active_users AS (
  SELECT user_id
  FROM ratings
  GROUP BY user_id
  HAVING COUNT(*) >= 50
),
exploded_genres AS (
  SELECT r.user_id, genre
  FROM ratings r
  JOIN active_users a ON r.user_id = a.user_id
  JOIN movies m ON m.movie_id = r.movie_id
  LATERAL VIEW explode(m.genres) AS genre
),
genre_counts AS (
  SELECT user_id, genre, COUNT(*) AS count
  FROM exploded_genres
  GROUP BY user_id, genre
),
ranked_genres AS (
  SELECT *, ROW_NUMBER() OVER (PARTITION BY user_id ORDER BY count DESC) AS rank
  FROM genre_counts
)
SELECT g.user_id, u.gender, u.occupation, g.genre, g.count
FROM ranked_genres g
JOIN users u ON g.user_id = u.user_id
WHERE rank = 1
ORDER BY g.count DESC
```

---

## Query (iv): Users Under 20

### Objective:
- Find users younger than 20

### Method:
- `WHERE age < 20`
- Sort by `age`

```sql
SELECT *
FROM users
WHERE age < 20
ORDER BY age
```

---

## Query (v): Scientists Aged 30–40

### Objective:
- Find users who are scientists and between 30 and 40 years old

```sql
SELECT *
FROM users
WHERE age BETWEEN 30 AND 40 AND occupation = 'scientist'
```

---

## Cassandra Tables Summary

| Table   | Description                                 |
|---------|---------------------------------------------|
| users   | user_id, age, gender, occupation, zip        |
| ratings | user_id, movie_id, rating, timestamp         |
| movies  | movie_id, title, genres (array<string>)      |

All tables were written using `.write.format("org.apache.spark.sql.cassandra")` and validated by `.read().show()` in Zeppelin.
