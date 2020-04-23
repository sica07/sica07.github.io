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
