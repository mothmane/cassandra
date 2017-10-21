## Cassandra Query Langage


#### TimeStamps

```
// create table user
CREATE TABLE user (first_name text,last_name text,PRIMARY KEY (first_name)) ;

// fill it with some data
INSERT INTO user (first_name , last_name ) VALUES ('Othmane','MANIAR');
INSERT INTO user (first_name , last_name ) VALUES ('Mougada','Jean');

// get all data in user
select * from user;

// add new column title
ALTER TABLE user ADD title text;

// get all data in user
select * from user;

// insert a new row
INSERT INTO user (first_name , last_name,title ) VALUES ('Miloudi','Adil','Mr');

```
#### TimeStamps

Timestamp is by column not row.

```
SELECT first_name, last_name from user;

// select with timestamp
SELECT first_name, last_name,writetime(last_name) from user;

// select with timestamp on primary key 
 SELECT first_name, last_name,writetime(first_name) from user; // ERROR

```

Update using timestamp take effect if timestamp is later than the effective timstamp on the column
`use with causion`

```
UPDATE user using TIMESTAMP 1508587000000000 SET last_name = 'Gabriel' WHERE first_name ='Mougada';

```

#### TTL (Time to live)

TTL is a feature in cassandra that make data expire when no longer needed
Forget the purge scripts it's made simple by using the time to live feature.

TTL default value is NULL (do not expire)
TTL is by column


```
SELECT first_name, last_name,TTL(last_name) from user;

SELECT first_name, last_name,TTL(first_name) from user; // ERROR when used on Primary Key
```

lets update a column specifiying 60 secondes for TTL

```
 UPDATE user USING TTL 60 SET last_name = 'Ludovic' WHERE first_name ='Mougada';

```
and then do a select on the table user

```

SELECT first_name, last_name,TTL(last_name) from user;
```
the result is :

| first_name | last_name | ttl(last_name)|
|------------|-----------|----------------|
|    Miloudi |      Adil |           `null`|
|    Mougada |   Ludovic |             43|
|    Othmane |    MANIAR |           `null`|

the TTL is decrementing .
Lets wait more than a minute an then reexecute the select :

the result is 
| first_name | last name | ttl(last_name)|
|------------|-----------|----------------|
|    Miloudi |      Adil |           `null`|
|    Mougada |      `null` |           `null`|
|    Othmane |    MANIAR |           `null`|

the lastname value for Mougada has desapeared .


Quite good isn't it? 

you can set a TTL only when you provide a value to a column 


