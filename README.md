# SQL

- [Create Table](#create_table)
- [Insert](#insert)
- [Query](#query)
- [Calculated Columns](#calculated_columns)
- [String Operators](#string_operator)
- [Filter](#filter)
- [Update](#update)
- [Delete](#delete)

#### <a name="create_table"></a>Create Table

```sql
CREATE TABLE cities (
    name VARCHAR(50),
    country VARCHAR(50),
    population INTEGER,
    area INTEGER
)
```

#### <a name="insert"></a>Insert

`one record`

```sql
INSERT INTO cities (name, country, population, area)
VALUES ('Tokyo', 'Japan', 38505000, 8233);
```

`more than one`

```sql
INSERT INTO cities (name, country, population, area)
VALUES
 ('Tokyo', 'Japan', 38505000, 8233),
 ('Shanghai', 'Chaina', 22125000, 4015),
 ('Sao', 'Paulo', 20935000, 3043);
```

#### <a name="query"></a>Query

```sql
SELECT * FROM cities
```

```sql
SELECT name, area FROM cities
```

`order optional`

```sql
SELECT area, name, population FROM cities
```

#### <a name="calculated_columns"></a>Calculated Columns

| Symbol | Operators   |
| :----- | :---------- |
| `+`    | Add         |
| `-`    | Subtract    |
| `*`    | Multiply    |
| `/`    | Divide      |
| `^`    | Exponent    |
| `l/`   | Square Root |
| `@`    | Absolute    |
| `%`    | Remainder   |

use `AS` new field name

```sql
SELECT name,population,area, population / area AS density
FROM cities
```

#### <a name="string_operator"></a>String Operators

| Symbol     | Operators                              |
| :--------- | :------------------------------------- |
| `ll`       | Join two strings                       |
| `CONCAT()` | Join two strings                       |
| `LOWER()`  | Gives a lower case string              |
| `LENGTH()` | Gives number of characters of a string |
| `UPPER()`  | Gives an uppper case string            |

use `||`

```sql
SELECT name || ', '|| country AS location
FROM cities

```

use `CONCAT`

```sql
SELECT CONCAT(name,', ', country) AS location
FROM cities

```

use `UPPER()` and `LOWER()`

```sql
SELECT UPPER(name) as name, LOWER(country) as country
FROM cities

```

use `LENGTH()`

```sql
SELECT LENGTH(name) as name_length
FROM cities

```

#### <a name="filter"></a>Filter

| Symbol    | Operators                                   |
| :-------- | :------------------------------------------ |
| `=`       | Are the values equal?                       |
| `>`       | Is the values on the left greater?          |
| `<`       | Is the values on the right less?            |
| `>=`      | Is the values on the left greater or equal? |
| `<=`      | Is the values on the right lesser or equal? |
| `IN`      | Is the value present in a list?             |
| `<>`      | Are the values not equal?                   |
| `!=`      | Are the values not equal?                   |
| `BETWEEN` | Is the value between two other values?      |
| `NOT IN`  | Is the value not present in a list?         |

##### Basic Where

use `>`

```sql
SELECT name, area
FROM cities
WHERE area > 4000
```

use `BETWEEN`

```sql
SELECT name, area
FROM cities
WHERE area BETWEEN 2000 AND 4000
```

use `IN`

```sql
SELECT name, area
FROM cities
WHERE name IN ('Tokyo','Shanghai')
```

use `NOT IN`

```sql
SELECT name, area
FROM cities
WHERE name NOT IN ('Tokyo','Shanghai')
```

use `NOT IN` + `and` + `=`

```sql
SELECT name,area
FROM cities
WHERE area NOT IN (3021,5000) AND name = 'Tokyo'
```

use `NOT IN` + `OR` + `=`

```sql
SELECT name,area
FROM cities
WHERE area NOT IN (3021,5000) OR name = 'Tokyo'
```

```sql
WHERE
  area NOT IN (3021,5000)
  OR name = 'Tokyo'
  OR name = 'Shanghai'
```

##### Calculations in Where

```sql
SELECT *
FROM cities
WHERE
  population / area > 6000
```

#### <a name="update"></a>Update

```sql
UPDATE cities
SET population = 39505000
WHERE name = 'Tokyo'
```

#### Delete

```sql
DELETE FROM cities
WHERE name = 'Tokyo'
```

#### Auto Generated ID

```sql
CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    username VARCHAR(50)
)
```

#### Create Foreign Key

``create``
```sql
CREATE TABLE photos (
    id SERIAL PRIMARY KEY,
    url VARCHAR(50),
    user_id INTEGER REFERENCES users(id)
)
```
``insert``
```sql
INSERT INTO photos(url, user_id)
VALUES ('/user/one.jpg',1 )
```
``query``
```sql
SELECT * FROM photos
```