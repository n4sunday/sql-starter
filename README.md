# SQL

- [Create Table](#create_table)
- [Insert](#insert)
- [Query](#query)
- [Calculated Columns](#calculated_columns)

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
