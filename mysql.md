# MySQL

```bash
aptitude install mysql-server mysql-client libmysqlclient-dev
vim /etc/mysql/my.cnf
```

## Force utf8mb4

```
[client]
default-character-set = utf8mb4

[mysql]
default-character-set = utf8mb4

[mysqld]
character-set-server = utf8mb4
collation-server = utf8mb4_unicode_ci
```

```bash
service mysql restart
```

## Add some PRIVILEGES for staging and/or production user
```
mysql -uroot -p
GRANT ALL PRIVILEGES ON  `%\_staging` . * TO  'staging'@'localhost' IDENTIFIED BY  '***';
GRANT ALL PRIVILEGES ON  `%\_production` . * TO  'production'@'localhost' IDENTIFIED BY  '***';
```