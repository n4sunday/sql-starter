#SQL


- [Create Table](#create_table)
- [Insert](#insert)


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
`ทore than one`
```sql
INSERT INTO cities (name, country, population, area)
VALUES
 ('Tokyo', 'Japan', 38505000, 8233),
 ('Shanghai', 'Chaina', 22125000, 4015),
 ('Sao', 'Paulo', 20935000, 3043);
```