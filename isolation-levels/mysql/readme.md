# Isolation levels in MySQL

## Run

Run MySQL + Adminer:

```bash
docker-compose up
```

## Connect

Use word `password` for the password:

```bash
docker exec -it isolation-levels-mysql mysql -uroot -p
```

## Create schema and insert data

```sql
CREATE SCHEMA simple_bank;

USE simple_bank;

CREATE TABLE `accounts`
(
    `id`         bigint PRIMARY KEY AUTO_INCREMENT,
    `owner`      varchar(255) NOT NULL,
    `balance`    bigint       NOT NULL,
    `currency`   varchar(255) NOT NULL,
    `created_at` datetime     NOT NULL DEFAULT (now())
);
```

```sql
INSERT INTO accounts (id, owner, balance, currency, created_at)
VALUES (1, 'one', 100, 'USD', '2020-09-06 15:09:38');

INSERT INTO accounts (id, owner, balance, currency, created_at)
VALUES (2, 'two', 100, 'USD', '2020-09-06 15:09:38');

INSERT INTO accounts (id, owner, balance, currency, created_at)
VALUES (3, 'three', 100, 'USD', '2020-09-06 15:09:38');
```

## Read

```sql
select * from accounts;
```

```terminal
+----+-------+---------+----------+---------------------+
| id | owner | balance | currency | created_at          |
+----+-------+---------+----------+---------------------+
|  1 | one   |     100 | USD      | 2020-09-06 15:09:38 |
|  2 | two   |     100 | USD      | 2020-09-06 15:09:38 |
|  3 | three |     100 | USD      | 2020-09-06 15:09:38 |
+----+-------+---------+----------+---------------------+
```

## Shutdown

Shutdown MySQL + Adminer:

```bash
docker-compose down
```

## Read isolation level of current session & global level

```mysql
USE simple_bank;

select @@transaction_isolation;
```

```terminal
+-------------------------+
| @@transaction_isolation |
+-------------------------+
| REPEATABLE-READ         |
+-------------------------+
```

This level is only applied to this specific MySQL console session. By default, it is repeatable read as we can see here.

There's also a global isolation level, which is applied to all sessions when they first started:

```sql
select @@global.transaction_isolation;
```

```terminal
+--------------------------------+
| @@global.transaction_isolation |
+--------------------------------+
| REPEATABLE-READ                |
+--------------------------------+
```