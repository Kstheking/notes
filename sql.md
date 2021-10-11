# sql = Structured Query Language

## SELECT  

- SELECT * FROM customers; 
- SELECT name, dob FROM customers;

## WHERE 

to filter columns we use SELECT something, something; to filter rows we use WHERE 

- SELECT * FROM products WHERE product_type='book';  (here product_type is a column in the table)

## Comparison operators 

| Operator  | Description |
| ------------- |:-------------:|
| =      | Equal to      |
| !=      | not equal     |
| <>     | not equal     |
| < | less than | 
| <= | less than or equal |
| > | greater than |
| >= | greater than or equal |  


- SELECT * FROM characters WHERE height > 180;

### AND 

to combine queries 

- SELECT * FROM characters WHERE gender='Female' AND hair='Black'

### OR 

to have |   `SELECT * FROM characters WHERE hair='Black' OR hair='Brown'`  

### IN 

to give an range of options  

`SELECT * FROM characters WHERE hair IN ('Black','Brown');`  

### NOT 

to flip the condition 

`SELECT * FROM characters WHERE eyes NOT IN ('blue','brown');`  

### Like 

using this we can use pattern matching 

- `_` means one character will match 
- `%` means any number of character will match   

`SELECT * FROM characters WHERE name LIKE '%IRON%';`  

### Distinct 

make result distinct by removing identical rows 

`SELECT DISTINCT nationality FROM characters;`

## ORDER BY 

order results by some column or attributes 

`SELECT * FROM characters ORDER BY height DESC;`  

- DESC reverses the order (no desc means ascending) 
- if you specify two columns then first it will sort by first column and then if equal by second column  `SELECT * FROM students ORDER BY gender, name;`

## CASE  

[microsoft docs on CASE](https://docs.microsoft.com/en-us/sql/t-sql/language-elements/case-transact-sql?view=sql-server-ver15)

- The CASE statement goes through conditions and returns a value when the first condition is met
- If no conditions are true, it returns the value in the ELSE clause. 
- If there is no ELSE part and no conditions are true, it returns NULL.

`SELECT name, CASE WHEN species = 'Human' THEN 'HUMAN' ELSE 'ALIEN' END FROM characters;` 

to specify column name use `END AS columnName`  or `columnName = CASE .... END`

```
CASE 
    WHEN .. THEN ...
    ELSE ...
END
```

## LIMIT 

- limit number of rows returned 
- must be specified at last, after ORDER_BY 
- `SELECT * FROM characters ORDER BY height ASC LIMIT 1;`
- for microsoft sql we have to use `SELECT TOP 50 FROM ...` way 

### COUNT 

- count the rows 
- SELECT COUNT(*) FROM characters;
- SELECT COUNT(*) FROM characters WHERE nationality='Asgardian';

### SUM 

- sum of a column
- we can't use wildcard with SUM (it will throw error even if there is only one column) 
- `SELECT SUM(budget) FROM movies;`

### AVG

- average 
- `SELECT AVG(budget) FROM movies;`
- can't use wildcard either

### MIN and MAX 

- min and max
- `SELECT MIN(budget), MAX(budget) FROM movies;`

## GROUP BY 

- groups rows by some attribute 
- if you just simply display the group by result, only one row (the last one) per group will be shown
- `SELECT nationality, COUNT(*) FROM characters GROUP BY nationality;`

## HAVING 

- sql runs in order FROM | JOINS > WHERE > GROUP BY 
- so if we want  to filter after GROUP BY we can't use WHERE 
- also we can't use WHERE on aggregate functions 
- therfore we need HAVING `SELECT nationality, COUNT(*) FROM characters GROUP BY nationality HAVING COUNT(id) > 1;`

## Handling null 

- Use COALESCE to replace the null values with appropriate default replacements 
- use conditions `IS NOT NULL` `IS NULL` 
- `SELECT * FROM characters WHERE alter_ego IS NOT NULL;`

## COALESCE 

- The COALESCE function lets us take several columns within one row of our data and return the first column that has a non-NULL value. If all columns specified are NULL then we can set a default value in the COALESCE function.
- `SELECT COALESCE(alter_ego, name) FROM characters;`

## Nested Query 

- `SELECT * FROM movies WHERE budget = (SELECT MIN(budget) FROM movies);`

##JOIN 

- for join we need primary key in one and foreign key in another which references the primary key in first one 

### Inner join 

- also known as simply JOIN 
- brings back records where primary and foreign key match 
- `SELECT * FROM movies INNER JOIN actors ON movies.lead_actor_id = actors.actor_id`  
- [on vs where](https://stackoverflow.com/questions/354070/sql-join-where-clause-vs-on-clause) although they may work same on INNER JOIN but they have different semantic meaning 

### LEft join 

- gets data from left table even though no match exists for it in the right table 
- `SELECT * FROM movies LEFT JOIN actors ON movies.lead_actor_id = actors.actor_id;`

### multiple joins 

- `SELECT * FROM movies INNER JOIN movie_actors_link ON movies.id=movie_actors_link.movie_id INNER JOIN actors ON movie_actors_link.actor_id = actors.actor_id`

### UNION 

- The UNION operator is used to combine the result-set of two or more SELECT statements.
- number of columns mentioned while UNION must be same
- datatype corresponding per column should be the same
- order should be the same 
- it also removes duplicates   
- to make sure duplicates are not removed use UNION ALL
- `SELECT name FROM cap_actors UNION SELECT name FROM avengers_actors`

## Alias 

### table alias 

`SELECT * FROM movies AS a INNER JOIN movie_actors_link AS  b ON a.id=b.movie_id INNER JOIN actors AS c ON b.actor_id = c.actor_id`

### column alias 

`SELECT movies.title as movie_name, actors.name as actor_name FROM movies as mv INNER JOIN movie_actors_link as mvac ON mv.id =mvac.movie_id INNER JOIN actors as ac ON mvac.actor_id = ac.actor_id;`

## self join 

`SELECT a.name as character_name, b.name as partner_name FROM characters as a INNER JOIN characters as b ON a.partner_id=b.id;`

## INSERT 

```
INSERT INTO table_name
VALUES (val1, val2, val3, ...);
```

```
INSERT INTO table_name (col1, col2, col3, ...)
VALUES (val1, val2, val3, ...)
```

## UPDATE 

- `UPDATE employees SET salary = salary * 1.1;`
- `UPDATE employees SET salary = salary * 1.1 WHERE grade='Manager';`

## DELETE 

- `DELETE FROM customers WHERE CustomerID = 6;`
