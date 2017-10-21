## START and STOP Cassandra 

#### Make sure you have rights to 
```sh
sudo chown -R <username> /var/lib/cassandra
```
#### Starting Server
```sh
<cassandra-directory>/bin/cassandra 
```
#### Stoping Server
```
Ctrl+C 
```
Or
```sh
pgrep -u <username> -f cassandra | xargs kill -9
```

## Running CQL Shell
```sh
<cassandra-directory>/bin/cqlsh
```
#### Running cql for specific server 
```sh
<cassandra-directory>/bin/cqlsh <host> <port>
```
#### Other way 

set `$CQLSH_HOST` and `CQLSH_PORT`
and 
```sh
<cassandra-directory>/bin/cqlsh 
```

## Basic CQLSH commands
```
HELP;
DESCRIBE CLUSTER;
DESCRIBE KEYSPACES;
SHOW VERSION;
```

#### Creating keyspaces and tables
```
CREATE KEYSPACE my_keyspace WITH 
```
 press Tab key
```
CREATE KEYSPACE my_keyspace WITH replication ={'class': '
```
Three possible replication stategies: 
  - NetworkTopologyStrategy
  - SimpleStragtegy
  - OldNetworkTopologyStrategy

press 's' then Tab key
```
CREATE KEYSPACE other WITH replication = {'class': 'SimpleStrategy' 
```
press Tab key
```
CREATE KEYSPACE other WITH replication = {'class': 'SimpleStrategy', 'replication_factor': 
```
choose 1 and then press Tab 
 
```
CREATE KEYSPACE other WITH replication = {'class': 'SimpleStrategy', 'replication_factor': 1};
```

## Manipulating keyspaces and tables

```
DESCRIBE KEYSPACE my_keyspace;
USE my_keyspace;
```

#### creating table
```
USE my_keyspace;
CREATE TABLE person ( first_name text, last_name text, PRIMARY KEY (first_name));
```
or 
```
CREATE TABLE my_keyspace.person ( first_name text, last_name text, PRIMARY KEY (first_name));
```

```
DESCRIBE TABLE person;
```



#### writes and reads 
```
INSERT INTO person(first_name , last_name ) VALUES ('Othmane','MANIAR');
SELECT * FROM person;
SELECT  COUNT(*) FROM person;
SELECT * FROM person WHERE first_name='Othmane';
```
```
SELECT * FROM person WHERE last_name='MANIAR';
InvalidRequest: Error from server: code=2200 [Invalid query] message="Cannot execute this query as it might involve data filtering and thus may have unpredictable performance. If you want to execute this query despite the performance unpredictability, use ALLOW FILTERING"
```
we will see why

```
DELETE last_name FROM person WHERE first_name ='Othmane';
DELETE  FROM person WHERE first_name ='Othmane';
```
```
TRUNCATE person;
```
```
DROP TABLE person;
```

you can check history of command by up and down keys
Or in hidden file .cassandra/cqlsh_history
