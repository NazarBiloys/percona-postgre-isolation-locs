# percona-postgre-isolation-locs
Table of existed a problem: `lost update`, `dirty read`,  `non-repeatable read`,  `phantom read` in InnoDB `percona`, `postgre`

```
  CREATE TABLE users (
  id INT NOT NULL,
  firstname VARCHAR(50) NOT NULL,
  lastname VARCHAR(50) NOT NULL,
  email VARCHAR(255) NOT NULL,
  password VARCHAR(255) NOT NULL,
  date_of_birth DATE NOT NULL,
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  PRIMARY KEY (id)
  );
```

```
  CREATE TABLE articles (
  id INT NOT NULL,
  title VARCHAR(50) NOT NULL,
  subject VARCHAR(50) NOT NULL,
  user_id INT NOT NULL,
  count INT NOT NULL,
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  PRIMARY KEY (id),
  FOREIGN KEY (user_id) REFERENCES users(id)
  );
```

`make some rows in this table`

## Results with different isolation level

#### Percona

`+` - Problem exist
`-` - Problem non exist

Use `DO SLEEP(seconds)` for trigger next problems.

|                                 | **Repeatable Read** | **Read Committed** | **Read Uncommitted** | **Serializable** |
|:-------------------------------:|:-------------------:|:------------------:|:--------------------:|:----------------:|
|        **Lost update**          |           +         |         +          |          +           |          -       |
|       **Dirty read**            |           -         |        -           |           +          |          -       |
|     **Non-repeatable read**     |           -         |        +           |           +          |          -       |
|     **Phantom read**            |           +         |         +          |           +          |          -       |


#### Postgre

|                                 | **Repeatable Read** | **Read Committed** | **Read Uncommitted** | **Serializable** |
|:-------------------------------:|:-------------------:|:------------------:|:--------------------:|:----------------:|
|        **Lost update**          |           +         |         +          |           +          |         -        |
|       **Dirty read**            |           -         |         -          |           -          |         -        |
|     **Non-repeatable read**     |           -         |         +          |           +          |         -        |
|     **Phantom read**            |           -         |         +          |           +          |         -        |
