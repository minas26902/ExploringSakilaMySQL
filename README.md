## Exploring the Sakila database in MySQL

HW#8 MySQL  Due Date - January 31, 2018

In this exercise I loaded the database 'sakila' into MySQL and ran queries using joins and subqueries when necessary to answer questions about the dataset. See SQL code below for each problem.

### Problems
    
    USE sakila;

1a.
    
    SELECT first_name, last_name
    FROM actor;

1b.
    
    SELECT upper(concat(first_name,' ', last_name)) AS actor_name
    FROM actor;

2a.
    
    SELECT actor_id, first_name, last_name
    FROM actor
    WHERE first_name LIKE 'Joe';

2b.
    
    SELECT actor_id, first_name, last_name
    FROM actor
    WHERE  last_name LIKE '%GEN%';

2c.
    
    SELECT actor_id, first_name, last_name
    FROM actor
    WHERE last_name LIKE '%LI%'
    ORDER by last_name, first_name;

2d.
    
    SELECT country_id, country
    FROM country
    WHERE country IN ('Afghanistan', 'Bangladesh', 'China');

3a.
    
    ALTER table actor
    ADD column middle_name VARCHAR(45) AFTER first_name;

3b.
    
    ALTER table actor
    MODIFY column middle_name blob;

3c.
    
    ALTER table actor
    DROP column middle_name;

4a.
    
    SELECT last_name, COUNT(last_name) AS counts
    FROM actor
    GROUP BY last_name
    HAVING (counts >= 1);

4b.
    
    SELECT last_name, COUNT(last_name) AS counts
    FROM actor
    GROUP BY last_name
    HAVING (counts > 1);

4c.
    
    UPDATE actor 
    SET first_name = 'Harpo'
    WHERE first_name = 'Groucho' AND last_name = 'Williams';

4d.
    
    UPDATE actor
    SET first_name = CASE
	    WHEN first_name = 'Harpo' THEN 'Groucho'
	    WHEN first_name = 'Groucho' THEN 'Mucho Groucho'
        ELSE first_name 
    END
    WHERE last_name = 'Williams';

5a.
    
    SHOW CREATE TABLE address;

6a.
    
    SELECT first_name, last_name, address
    FROM staff s
    JOIN address a
    USING (address_id);

6b.
    
    SELECT first_name, last_name, concat('$', format(SUM(amount),2)) AS total_rung
    FROM staff s
    JOIN payment p
    USING (staff_id)
    WHERE DATE(payment_date) LIKE '2005-08%'
    GROUP BY first_name, last_name;

6c.
    
    SELECT title, COUNT(actor_id) AS count
    FROM film
    JOIN film_actor
    USING(film_id)
    GROUP BY title;

6d.
    
    SELECT title, COUNT(inventory_id) AS copies
    FROM film
    JOIN inventory
    USING (film_id)
    WHERE title = 'Hunchback Impossible'
    GROUP BY title;

6e.
    
    SELECT last_name, concat('$', format(sum(amount),2)) AS total_paid
    FROM customer
    JOIN payment
    USING (customer_id)
    GROUP BY last_name
    ORDER BY last_name;

7a.
    
    SELECT language_id, title
    FROM film
    WHERE title LIKE 'Q%' OR title LIKE 'K%'
    AND language_id IN
	(
	 SELECT language_id
	 FROM language
	 WHERE name = 'English'
    );

7b.
    
    SELECT first_name, last_name
    FROM actor
    WHERE actor_id IN
    (
     SELECT actor_id
     FROM film_actor
     WHERE film_id IN
     (
      SELECT film_id 
      FROM film
      WHERE title = 'Alone Trip'
     )
    );

7c.
    
    SELECT c.customer_id, c.first_name, c.last_name, c.email, co.country 
    FROM customer c, address a, city ci, country co 
    WHERE c.address_id = a.address_id 
    AND a.city_id = ci.city_id 
    AND ci.country_id = co.country_id 
    AND co.country ='Canada';

7d.
    
    SELECT title
    FROM film 
    WHERE film_id IN
    (
     SELECT film_id
     FROM film_category 
     WHERE category_id IN
     (
      SELECT category_id
      FROM category c
      WHERE name = 'Family'
     )
    );

7e.
    
    SELECT title, COUNT(rental_id) AS count
    FROM film f, inventory i, rental r
    WHERE f.film_id = i.film_id
    AND i.inventory_id = r.inventory_id
    GROUP BY title
    ORDER BY title DESC;

7f.
    
    SELECT s.store_id, concat('$', format(SUM(amount),2)) AS 'total_business'
    FROM store s, staff sf, payment p
    WHERE s.store_id = sf.store_id
    AND sf.staff_id = p.staff_id
    GROUP BY s.store_id;

7g.
    
    SELECT s.store_id, city, country
    FROM store s, address a, city ci, country co
    WHERE s.address_id = a.address_id
    AND a.city_id = ci.city_id
    AND ci.country_id = co.country_id;

7h.
    
    SELECT c.name, concat ('$', format(SUM(amount), 2)) AS 'Gross Revenue' 
    FROM category c, film_category fc, inventory i, rental r,  payment p
    WHERE c.category_id = fc.category_id
    AND fc.film_id = i.film_id
    AND i.inventory_id = r.inventory_id
    AND r.rental_id = p.rental_id
    GROUP BY c.name
    ORDER BY SUM(amount) DESC
    LIMIT 5;

8a.
    
    CREATE VIEW view1 AS
    SELECT c.name, concat ('$', format(SUM(amount), 2)) AS 'Gross Revenue' 
    FROM category c, film_category fc, inventory i, rental r,  payment p
    WHERE c.category_id = fc.category_id
    AND fc.film_id = i.film_id
    AND i.inventory_id = r.inventory_id
    AND r.rental_id = p.rental_id
    GROUP BY c.name
    ORDER BY SUM(amount) DESC
    LIMIT 5;

8b.
    
    SELECT * FROM view1;

8c.
    
    DROP VIEW view1;
    
