# SQL assignment

## Create and Insert Statements for SQL tables Q1

### **Authors table**

#### Create table
```sql
CREATE TABLE authors (
	id SERIAL,
	name VARCHAR(50)
);
```
#### Insert statement
```sql
INSERT INTO authors(name) VALUES ('JK Rowling'), ('Stephen King'), ('Amy Tan'), ('George R.R Martin');
```

### **Books table**

#### Create table

```sql
CREATE TABLE books (
	id SERIAL,
	title VARCHAR(100),
	author_id INTEGER
);
```

#### Insert statement

```sql
INSERT INTO books(title, author_id) VALUES ('Harry Potter and the Cursed Child', 1), ('Harry Potter and the Goblet of Fire', 1), ('It', 2), ('Doctor Sleep', 2), ('The Moon Lady', 3), ('The Joy Luck Club', 3);
```

### **Reviews table**

#### Create table

```sql
CREATE TABLE reviews (
	id SERIAL,
	rating INTEGER,
	reviewer_id INTEGER,
	book_id INTEGER
);
```

#### Insert statement

```sql
INSERT INTO reviews(rating, reviewer_id, book_id) VALUES (3, 2, 1), (9, 1, 6), (5, 3, 3);
```

# SQL assignment Answers

## Q1.1

```sql
SELECT books.title AS book_title, authors.name AS author_name
FROM authors
JOIN books ON authors.id = books.author_id;
```

## Q1.2a

```sql
SELECT books.title AS book_title, authors.name AS author_name
FROM authors
LEFT JOIN books ON authors.id = books.author_id;
```

## Q1.2b

```sql
SELECT books.title AS book_title, authors.name AS author_name
FROM authors
FULL JOIN books ON authors.id = books.author_id;
```

## Q1.3

```sql
SELECT books.title, authors.name AS author_name, reviews.rating
FROM authors
JOIN books ON authors.id = books.author_id
JOIN reviews ON reviews.book_id = books.id
WHERE authors.id = reviews.reviewer_id;
```

## Q1.4

```sql
SELECT authors.id AS author_id, COUNT(books.id) AS books_authored
FROM authors
LEFT JOIN books on authors.id = books.author_id
GROUP BY authors.id
ORDER BY author_id;
```

## Q1.5

```sql
SELECT authors.name AS author_name, COUNT(books.id) AS books_authored
FROM authors
LEFT JOIN books on authors.id = books.author_id
GROUP BY authors.name
ORDER BY author_name;
```

## Create and Insert Statements for SQL tables Q2

### **Phones table**

#### Create table
```sql
CREATE TABLE phones (
	name VARCHAR(50),
	manufacturer VARCHAR(100),
	price INTEGER,
	units_sold INTEGER
);
```

#### Insert statement
```sql
INSERT INTO phones VALUES ('iPhone 12 Pro', 'Apple', 999, 5001), ('iPhone 13 Pro', 'Apple', 1049, 201), ('Galaxy S21 Ultra', 'Samsung', 769, 100), ('Pixel 6', 'Google', 599, 334), ('G5', 'Motorola', 150, 100 );
```

## Q2.1

```sql
SELECT * FROM (
	SELECT manufacturer, SUM(price*units_sold) AS total_revenue
	FROM phones
	GROUP BY manufacturer
) AS revenue
WHERE revenue.total_revenue > 200000
ORDER BY total_revenue DESC;
```

## Q2.2

```sql
SELECT name
FROM phones
ORDER BY price DESC
LIMIT 2 OFFSET 1;
```

## Q2.3

```sql
SELECT * FROM 
	(
		SELECT manufacturer 
		FROM phones
		WHERE price < 170 
		UNION
		SELECT manufacturer FROM 
			(
				SELECT manufacturer, COUNT(manufacturer) AS phones_cnt
				FROM phones
				GROUP by manufacturer
			) AS phones_agg
		WHERE phones_cnt >= 2
	) AS manufacturers
```

## Q2.4

```sql
SELECT name, price, price::REAL/(SELECT MAX(price)
FROM phones) AS price_ratio
FROM phones;
```

## Q2.5

```sql
SELECT name, price
FROM phones
WHERE units_sold > 5000;
```

## Q2.6

```sql
SELECT name, manufacturer
FROM phones
WHERE manufacturer IN ('Apple', 'Samsung');
```

## Q2.7

```sql
SELECT * FROM (SELECT name, price*units_sold AS total_revenue
FROM phones) as rev_tbl
WHERE total_revenue > 100000;
```

## Q3.1

```sql
SELECT 
	COUNT(CASE WHEN paid = true then 1 ELSE NULL END) AS paid_orders,
	COUNT(CASE WHEN paid = false then 1 ELSE NULL END) AS unpaid_orders
FROM orders
```

## Q3.2

```sql
SELECT first_name, last_name, paid
FROM users
JOIN orders ON users.id = orders.user_id;
```
