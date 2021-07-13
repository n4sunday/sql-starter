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

#### Relation

##### Create Foreign Key

`create`

```sql
CREATE TABLE photos (
    id SERIAL PRIMARY KEY,
    url VARCHAR(50),
    user_id INTEGER REFERENCES users(id)
)
```

`insert`

```sql
INSERT INTO photos(url, user_id)
VALUES
    ('/user/one.jpg',1 ),
    ('/user/two.jpg',2)
```

`query`

```sql
SELECT * FROM photos
```

##### Running Queries on Associated Data

```sql
SELECT url, username FROM photos
JOIN users ON users.id = photos.user_id
```

##### Delete Cascade

`ON DELETE CASCADE`

```sql
CREATE TABLE photos (
    id SERIAL PRIMARY KEY,
    url VARCHAR(200),
    user_id INTEGER REFERENCES users(id) ON DELETE CASCADE
);
```

##### Delete Null

`ON DELETE SET NULL`

```sql
CREATE TABLE photos (
  id SERIAL PRIMARY KEY,
  url VARCHAR(200),
  user_id INTEGER REFERENCES users(id) ON DELETE SET NULL
);
```

#### One-to-One Relationship

```bash
Boat
   ├── Crew member
   ├── Crew member
   └── Crew member
```

a boat has `many crew` member
a crew member `has one` boat

##### Auto Generate ID

user `SERIAL`

```sql
CREATE TABLE users (
  id SERIAL PRIMARY KEY,
  username VARCHAR(50)
)
```

##### Creating Foreign Keys Columns

use `REFERENCES` table(foreign key)

```sql
CREATE  TABLE photos (
    id SERIAL PRIMARY KEY,
    url VARCHAR(200),
    user_id INTEGER REFERENCES users(id)
)
```

##### Runnig Queries on Associated Data

Find all the photo created by user with id 1

```sql
SELECT * FROM photos
WHERE  user_id = 1
```

List all photos with details about the associated user for each

```sql
SELECT url, username FROM photos
JOIN users ON users.id = photos.user_id
```

##### Constratints Around Deletion

| On Delete Option        | What happens                                                        |
| :---------------------- | :------------------------------------------------------------------ |
| `ON DELETE RESTRICT`    | Throw an error                                                      |
| `ON DELETE NO ACTION`   | Throw an error                                                      |
| `ON DELETE NO CASCADE`  | Delete the photo too!                                               |
| `ON DELETE SET NULL`    | Set the foreign key of table to 'NULL'                              |
| `ON DELETE SET DEFAULT` | Set the foreign key of table to a default value, if one is provided |

```sql
CREATE TABLE photos (
  id SERIAL PRIMARY KEY,
  url VARCHAR(200),
  user_id INTEGER REFERENCES users(id) ON DELETE SET NULL
);
```

#### Joining Data from Different Tables

```sql
CREATE TABLE comments (
  id SERIAL PRIMARY KEY ,
  contents VARCHAR(250) ,
  user_id INTEGER REFERENCES users(id),
  photo_id INTEGER REFERENCES photos(id)
)

CREATE TABLE users (
  id SERIAL PRIMARY KEY,
  username VARCHAR(50)
)

INSERT INTO users (username)
VALUES
       ('Luke'),
       ('Solo'),
       ('Obiwan'),
       ('Vader'),
       ('Boba');

INSERT INTO photos (url, user_id)
VALUES
    ('test1',4),
    ('test2',4),
    ('test3',5),
    ('test4',5),
    ('test6',7),
    ('test7',7),
    ('test5',6),
    ('test8',6),
    ('test9',8);

INSERT INTO comments (contents, user_id, photo_id)
VALUES
    ('Go',4,8),
    ('To',4,8),
    ('Go',4,9),
    ('Go',5,10),
    ('Go',6,7);

SELECT contents, username
FROM comments
JOIN users ON comments.user_id = users.id;
```

#### Alternate Forms of Syntax

```sql
SELECT comments.id AS comment_id, photos.id AS photo_id
FROM photos
JOIN  comments ON photos.id = comments.photo_id;

SELECT comments.id AS comment_id, p.id AS photo_id
FROM photos AS p
JOIN  comments ON p.id = comments.photo_id;

```

#### Aggregating of Record
```sql
SELECT user_id
FROM comments
GROUP BY user_id
```
