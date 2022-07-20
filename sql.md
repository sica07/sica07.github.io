# SQL
[<TIL](Programming.md)

##Setting a foreign key
Example:
```sql
    CREATE TABLE dispatcher_template (
        template_id int unsigned auto_increment primary key,
        zm_id int unsigned not null,
        pool_id int unsigned not null,
        FOREIGN KEY fk_zm(zm_id) REFERENCES zm(id_zm) ON UPDATE CASCADE ON DELETE RESTRICT,
        FOREIGN KEY fk_pool(pool_id) REFERENCES dispatcher_pool(pool_id) ON UPDATE CASCADE ON DELETE RESTRICT
    ) ENGINE=InnoDB;
```
#### Mandatory
* **Engine** should be the same e.g. InnoDB
* **Datatype** should be the same, and with same length. e.g. VARCHAR(20)
* Collation Columns **charset** should be the same. e.g. utf8 (Watchout: Even if your tables have same Collation, columns still could have different one.)
* **Unique** - Foreign key should refer to field that is unique (usually primary key) in the reference table.
* **Unsigned** - both columns should be of this type

#### Debugging
Run this in mysql console: `SHOW ENGINE INNODB STATUS;` and look in the _LATEST FOREIGN KEY ERROR_ section.
Also, you should run the query `set foreign_key_checks=0` before running the DDL so you can create the tables in an arbitrary order rather than needing to create all parent tables before the relevant child tables.


## Show Create Statement for a table
```sql
> show create table users\G
*************************** 1. row ***************************
       Table: users
Create Table: CREATE TABLE `users` (
  `id` mediumint(8) unsigned NOT NULL AUTO_INCREMENT,
  `first_name` varchar(80) NOT NULL,
  `last_name` varchar(80) NOT NULL,
  `email` varchar(80) NOT NULL,
  `middle_initial` varchar(80) NOT NULL,
  PRIMARY KEY (`id`),
  UNIQUE KEY `unique_email` (`email`)
) ENGINE=InnoDB DEFAULT CHARSET=latin1
```

[source](https://github.com/jbranchaud/til/blob/master/mysql/show-create-statement-for-a-table.md)


## Show indexes for a table
```sql
> show indexes in users;
+-------+------------+--------------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+
| Table | Non_unique | Key_name     | Seq_in_index | Column_name | Collation | Cardinality | Sub_part | Packed | Null | Index_type | Comment | Index_comment |
+-------+------------+--------------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+
| users |          0 | PRIMARY      |            1 | id          | A         |           0 |     NULL | NULL   |      | BTREE      |         |               |
| users |          0 | unique_email |            1 | email       | A         |           0 |     NULL | NULL   |      | BTREE      |         |               |
+-------+------------+--------------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+
```

[source](https://github.com/jbranchaud/til/blob/master/mysql/show-indexes-for-a-table.md)

## Profiling mysql queries

### profiling method

```
  mysql> UPDATE performance_schema.setup_instruments
      SET ENABLED = 'YES', TIMED = 'YES'
      WHERE NAME LIKE '%statement/%';

  mysql> UPDATE performance_schema.setup_instruments
      SET ENABLED = 'YES', TIMED = 'YES'
      WHERE NAME LIKE '%stage/%';

  mysql> UPDATE performance_schema.setup_consumers
      SET ENABLED = 'YES'
      WHERE NAME LIKE '%events_statements_%';

  mysql> UPDATE performance_schema.setup_consumers
      SET ENABLED = 'YES'
      WHERE NAME LIKE '%events_stages_%';
```

```
  mysql> SELECT * FROM employees.employees WHERE emp_no = 10001;
```

```
  mysql> SELECT EVENT_ID, TRUNCATE(TIMER_WAIT/1000000000000,6) as Duration, SQL_TEXT
      FROM performance_schema.events_statements_history_long WHERE SQL_TEXT like '%10001%';

       +----------+----------+--------------------------------------------------------+
       | event_id | duration | sql_text                                               |
       +----------+----------+--------------------------------------------------------+
       |       31 | 0.028310 | SELECT * FROM employees.employees WHERE emp_no = 10001 |
       +----------+----------+--------------------------------------------------------+
```

```
       mysql> SELECT event_name AS Stage, TRUNCATE(TIMER_WAIT/1000000000000,6) AS Duration
      FROM performance_schema.events_stages_history_long WHERE NESTING_EVENT_ID=31;
       +--------------------------------+----------+
       | Stage                          | Duration |
       +--------------------------------+----------+
       | stage/sql/starting             | 0.000080 |
       | stage/sql/checking permissions | 0.000005 |
       | stage/sql/Opening tables       | 0.027759 |
       | stage/sql/init                 | 0.000052 |
       | stage/sql/System lock          | 0.000009 |
       | stage/sql/optimizing           | 0.000006 |
       | stage/sql/statistics           | 0.000082 |
       | stage/sql/preparing            | 0.000008 |
       | stage/sql/executing            | 0.000000 |
       | stage/sql/Sending data         | 0.000017 |
       | stage/sql/end                  | 0.000001 |
       | stage/sql/query end            | 0.000004 |
       | stage/sql/closing tables       | 0.000006 |
       | stage/sql/freeing items        | 0.000272 |
       | stage/sql/cleaning up          | 0.000001 |
       +--------------------------------+----------+
```

```
    mysql> UPDATE performance_schema.setup_consumers
          SET ENABLED = 'NO'
          WHERE NAME LIKE '%events_statements_%';

   mysql> UPDATE performance_schema.setup_consumers
          SET ENABLED = 'NO'
          WHERE NAME LIKE '%events_stages_%';
```

[source](https://dev.mysql.com/doc/refman/8.0/en/performance-schema-query-profiling.html)

### Mysql profiling (will be deprecated)
The SQL Profiler is built into the database server and can be dynamically enabled/disabled via the MySQL client utility.
To begin profiling one or more SQL queries, simply issue the following command:

  `mysql> set profiling=1;`

Two things happen once you issue this command. First, any query you issue from this point on will be traced by the server with
various performance diagnostics being created and attached to each distinct query. Second, a memory table named profiling
is created in the INFORMATION_SCHEMA database for your particular session (not viewable by any other MySQL session)
that stores all the SQL diagnostic results. This table remains persistent until you disconnect from MySQL at which point it is destroyed.
Now, simply execute a SQL query:

`mysql> select count(*) from client where broker_id=2;`

```
    mysql> show profiles;

      +----------+------------+-----------------------------------------------+
      | Query_ID | Duration   | Query                                         |
      +----------+------------+-----------------------------------------------+
      |        0 | 0.00007300 | set profiling=1                               |
      |        1 | 0.00044700 | select count(*) from client where broker_id=2 |
      +----------+------------+-----------------------------------------------+
```

```
      mysql> show profile for query 1;

      +--------------------+------------+
      | Status             | Duration   |
      +--------------------+------------+
      | (initialization)   | 0.00006300 |
      | Opening tables     | 0.00001400 |
      | System lock        | 0.00000600 |
      | Table lock         | 0.00001000 |
      | init               | 0.00002200 |
      | optimizing         | 0.00001100 |
      | statistics         | 0.00009300 |
      | preparing          | 0.00001700 |
      | executing          | 0.00000700 |
      | Sending data       | 0.00016800 |
      | end                | 0.00000700 |
      | query end          | 0.00000500 |
      | freeing items      | 0.00001200 |
      | closing tables     | 0.00000800 |
      | logging slow query | 0.00000400 |
      +--------------------+------------+
```

```
      mysql> select min(seq) seq,state,count(*) numb_ops,
      -> round(sum(duration),5) sum_dur, round(avg(duration),5) avg_dur,
      -> round(sum(cpu_user),5) sum_cpu, round(avg(cpu_user),5) avg_cpu
      -> from information_schema.profiling
      -> where query_id = 7
      -> group by state
      -> order by seq;
      -------+----------------------+----------+---------+---------+---------+---------+
      | seq   | state                | numb_ops | sum_dur | avg_dur | sum_cpu | avg_cpu |
      +-------+----------------------+----------+---------+---------+---------+---------+
      |     0 | (initialization)     |        1 | 0.00004 | 0.00004 | 0.00000 | 0.00000 |
      |     1 | Opening tables       |        1 | 0.00023 | 0.00023 | 0.00000 | 0.00000 |
      |     2 | System lock          |        1 | 0.00001 | 0.00001 | 0.00000 | 0.00000 |
      |     3 | Table lock           |        1 | 0.00001 | 0.00001 | 0.00000 | 0.00000 |
      |     4 | checking permissions |        1 | 0.00010 | 0.00010 | 0.00000 | 0.00000 |
      |     5 | optimizing           |        4 | 0.00004 | 0.00001 | 0.00000 | 0.00000 |
      |     6 | statistics           |        4 | 0.00007 | 0.00002 | 0.00100 | 0.00025 |
      |     7 | preparing            |        4 | 0.00005 | 0.00001 | 0.00000 | 0.00000 |
      |     8 | Creating tmp table   |        1 | 0.00003 | 0.00003 | 0.00000 | 0.00000 |
      |     9 | executing            |    37352 | 0.16631 | 0.00000 | 0.05899 | 0.00000 |
      |    10 | Copying to tmp table |        1 | 0.00006 | 0.00006 | 0.00000 | 0.00000 |
      |    15 | Sending data         |    37353 | 3.85151 | 0.00010 | 3.72943 | 0.00010 |
      | 74717 | Sorting result       |        1 | 0.00112 | 0.00112 | 0.00100 | 0.00100 |
      | 74719 | removing tmp table   |        2 | 0.00003 | 0.00001 | 0.00000 | 0.00000 |
      | 74721 | init                 |        1 | 0.00002 | 0.00002 | 0.00000 | 0.00000 |
      | 74727 | end                  |        1 | 0.00001 | 0.00001 | 0.00000 | 0.00000 |
      | 74728 | query end            |        1 | 0.00000 | 0.00000 | 0.00000 | 0.00000 |
      | 74729 | freeing items        |        1 | 0.00002 | 0.00002 | 0.00000 | 0.00000 |
      | 74730 | closing tables       |        2 | 0.00001 | 0.00001 | 0.00000 | 0.00000 |
      | 74733 | logging slow query   |        1 | 0.00000 | 0.00000 | 0.00000 | 0.00000 |
      +-------+----------------------+----------+---------+---------+---------+---------+
```


[source](http://web.archive.org/web/20110609054749/http://dev.mysql.com/tech-resources/articles/using-new-query-profiler.html)

## When to use SQLite

The point of SQLite is a replacement for generating a file format. Although it's a database,
it lets us (developers) re-use our knowledge of databases when doing basic file I/O.
Think of SQLite as a file format which happens to have a query interface, and not a database.

SQLite really is a "file-format with a query language" rather than a "small database"

"SQLite does not compete with client/server databases. SQLite competes with fopen()."

If your data-access scenario is read-mostly write-sporadically, and your data being in a single place is ok, SQLite is fine.

An embedded DBMS is geared towards serving a single purpose-built client, which is great for a desktop application that wants a reliable way to store user data.
Once you have multiple clients being developed and running concurrently, and you have production data (customer accounts that are effectively legal documents that must be preserved at all times) you want that DBMS to be an independent component. It's not principally about the concurrent performance, rather it's the administrative tasks.
That requires a level of configuration and control that is contrary to the mission of SQLite to be embedded. They don't, and shouldn't, add that kind of functionality.

It also doesn't have strong/static typing (it's dynamically typed) so you have to typecheck your inputs or do type coercion upon read.
And it doesn't have a native date type. Date handling has to be handled at the application layer. It can be tricky to do massive time-series calculations or date-based aggregations.
You can use integers or text types to represent dates, but this open-endedness means you can't share your db because everyone implements their own datetime representation.

[source 1](https://news.ycombinator.com/item?id=23281994)
[source 2](https://www.sqlite.org/whentouse.html)
[source 3](https://news.ycombinator.com/item?id=26581170)
[source 4](https://news.ycombinator.com/item?id=26581891)

## Sqlite awesome capabilities

- create tables from csv files: `.import --csv city.csv city`
- export data as csv or json:
```
.mode json
.output city.json
select city, foundation_year, timezone from city limit 10;
.shell cat city.json`
```
- analize and work with json files:
```
select
  json_extract(value, '$.iso.code') as code,
  json_extract(value, '$.iso.number') as num,
  json_extract(value, '$.name') as name,
  json_extract(value, '$.units.major.name') as unit
from
  json_each(readfile('currency.sample.json'))
;
```
[source](https://antonz.org/sqlite-is-not-a-toy-database)

## Sqlite one line csv operations

`$sqlite3 :memory: -cmd '.mode csv' -cmd '.import taxi.csv taxi' -cmd '.mode column' \`
`'SELECT passenger_count, COUNT(*), AVG(total_amount) FROM taxi GROUP BY passenger_count'`

[source](https://til.simonwillison.net/sqlite/one-line-csv-operations)

### Soft deletion done right
**Opinion 1:**

The author is arguing against the use of "deleted" bool column to indicate
deletion. His solution of moving deleted objects to their own table gives
you the ability to un-delete, just as before. Only now, your queries and
indexes are simpler and you get to use foreign keys and other useful futures.

[src](https://news.ycombinator.com/item?id=32158162)

**Opinion 2:**

I just wanted to touch on the fact that eliding soft-deleted rows from
queries is really, really easy - this article makes it out to be a
constant headache but here's my suggested approach.
```
    ALTER TABLE blah ADD COLUMN deleted_at NULL TIMESTAMP;
    ALTER TABLE blah RENAME TO blahwithdeleted;
    CREATE VIEW blah (SELECT * FROM blahwithdeleted WHERE deleted_at IS NULL);
```
And thus your entire application just needs to keep SELECTing from blah
while only a few select pieces of code related to undeleting things
(or generating reports including deleted things) need to be shifted to
read from blahwithdeleted.

[src](https://news.ycombinator.com/item?id=32156461)
