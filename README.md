# Index, view, trigger, procedure, function, transaction

Show index

```bash
SHOW INDEX FROM table_name
```

Explain index

```bash
EXPLAIN SELECT * FROM table_name WHERE price = 9990 and category = 'Category name';
```

Add index

```bash
ALTER TABLE products ADD INDEX price_category (price,category);
```

Create View

```bash
CREATE
    ALGORITHM = UNDEFINED
    DEFINER = `root`@`localhost`
    SQL SECURITY DEFINER
VIEW `db_name`.`staff_list` AS
SELECT s.staff_id AS ID,
    concat(first_name, ' ', last_name) AS name,
    a.address,
    s.email,
    a.phone,
    a.postal_code As 'zip code',
    c.city,
    country.country
FROM staff as s
JOIN address as a
USING (address_id)
JOIN city as c
USING (city_id)
JOIN country
USING(country_id)
```

Create trigger

```bash
DELIMITER $$
CREATE TRIGGER check_sum BEFORE UPDATE ON table_name
FOR EACH ROW
   BEGIN
       IF NEW.balance < 0 THEN
           SET NEW.balance = 0;
       ELSEIF NEW.balance > 10000 THEN
           SET NEW.balance = 9999;
       END IF;
   END;$$
DELIMITER ;
```

Create procedure

```bash
DROP PROCEDURE IF EXISTS find_cities;

DELIMITER $$

CREATE PROCEDURE find_cities(IN city_code varchar(3))
BEGIN
  SELECT * FROM city
  WHERE CountryCode = city_code;
END$$

DELIMITER ;

CALL find_cities('AFG');
```

Create procedure

```bash
DROP PROCEDURE IF EXISTS count_cities;

DELIMITER $$

CREATE PROCEDURE count_cities(IN city_code varchar(3), OUT amount SMALLINT)
BEGIN
  SELECT count(*) INTO amount
  FROM city WHERE CountryCode = city_code;
END$$

DELIMITER ;

CALL count_cities('AFG', @amount);
SELECT @amount;
```

Create function

```bash

DROP FUNCTION IF EXISTS count_cities;

DELIMITER $$

CREATE FUNCTION count_cities(city_code varchar(3))
RETURNS SMALLINT
DETERMINISTIC
BEGIN
  DECLARE amount SMALLINT DEFAULT 0;
  SELECT count(*) INTO amount
  FROM city
  WHERE CountryCode = city_code;
  RETURN amount;
END$$

DELIMITER ;

SELECT count_cities('AFG');
```

Show existing procedures

```bash
SELECT
ROUTINE_NAME,
ROUTINE_DEFINITION
FROM information_schema.ROUTINES
WHERE ROUTINE_SCHEMA = 'database_name' AND
ROUTINE_TYPE = 'PROCEDURE';
```

Create transaction

```bash
START TRANSACTION;

UPDATE balance
SET sum = sum - 100
WHERE user_id = 1;

UPDATE balance
SET sum = sum + 100
WHERE user_id = 2;

COMMIT;
```

Create transaction using FOR UPDATE

```bash
START TRANSACTION;

SELECT sum FROM balance
WHERE user_id = 1 FOR UPDATE;

UPDATE balance
SET sum = sum + 100
WHERE user_id = 1;

COMMIT;
```

Levels of isolation

```bash
READ UNCOMMITTED
READ COMMITTED
REPEATABLE READ
SERIALIZABLE
```
