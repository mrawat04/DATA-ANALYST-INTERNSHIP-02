# DATA-ANALYST-INTERNSHIP-02
This repository contains SQL scripts for handling missing values, checking duplicates, and formatting dates in the Sakila sample database. The work focuses on data cleaning and preparation for analysis, ensuring data integrity and readability.

Contents
1. Handling Missing Values

Some columns in the Sakila database contain NULL or missing values. The scripts provide strategies to handle them:

Film Table

Columns like original_language_id may be NULL if a movie was produced in its default language.

Missing values here are valid and do not require updates.

SELECT * FROM sakila.film
WHERE film_id IS NULL
   OR title IS NULL
   OR language_id IS NULL
   OR original_language_id IS NULL;


Address Table

Missing phone numbers are replaced with "Not Provided".

Missing postal_code values are replaced with 0.

SET SQL_SAFE_UPDATES = 0;

UPDATE sakila.address
SET postal_code = 0
WHERE postal_code IS NULL OR postal_code = '';

UPDATE sakila.address
SET phone = 'Not Provided'
WHERE phone IS NULL OR phone = '';


Staff Table

Missing picture values are replaced with 'NONE'.

Missing password values are replaced with 'None'.

UPDATE sakila.staff
SET picture='NONE'
WHERE picture IS NULL;

UPDATE sakila.staff
SET password='None'
WHERE password IS NULL;

2. Checking Duplicates

Some columns may contain duplicate values, which may or may not need to be handled depending on business logic:

Actor Table: Duplicate last_name values are valid.

Rental Table: Duplicate inventory_id values are valid because a single DVD can be rented multiple times.

-- Checking duplicate last names
SELECT last_name, COUNT(*)
FROM sakila.actor
GROUP BY last_name
HAVING COUNT(*) > 1;

-- Checking duplicate inventory IDs
SELECT inventory_id, COUNT(*)
FROM sakila.rental
GROUP BY inventory_id
HAVING COUNT(*) > 1;

3. Date Formatting

All date columns are formatted to dd-mm-yyyy for consistency and readability:

-- Customer creation date
SELECT DATE_FORMAT(create_date, '%d-%m-%Y') AS formatted_date
FROM sakila.customer;

-- Payment date
SELECT DATE_FORMAT(payment_date, '%d-%m-%Y') AS formatted_date
FROM sakila.payment;

-- Rental and return dates
SELECT DATE_FORMAT(rental_date, '%d-%m-%Y') AS formatted_date
FROM sakila.rental;

SELECT DATE_FORMAT(return_date, '%d-%m-%Y') AS formatted_date
FROM sakila.rental;

Notes

All column headers and data types are correct and consistent.

The repository focuses on cleaning, validating, and formatting Sakila database tables to make them ready for analysis.

This is safe to use for data analytics or reporting purposes.
