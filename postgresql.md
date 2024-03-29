# PostgreSQL on Ubuntu 18.04 (with APT)

NOTICE: Use https://www.postgresql.org/download/linux/ubuntu/ to find proper installation instructions

## Step 1: install postgressql-12.xx

```bash
wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -
sh -c 'echo deb http://apt.postgresql.org/pub/repos/apt/ bionic-pgdg main > /etc/apt/sources.list.d/pgdg.list'
apt update
apt install postgresql-12 libpq-dev
```

## Step 2: tune PostgreSQL

 * Visit: http://pgtune.leopard.in.ua/ and enter Parameters of your system
 * Modify `/etc/postgresql/12/main/postgresql.conf` according values from pgtune or use `ALTER SYSTEM` to generate `/var/lib/postgresql/12/main/postgresql.auto.conf`
 * `service postgresql restart`
 * Create Database Users https://www.digitalocean.com/community/tutorials/how-to-use-postgresql-with-your-ruby-on-rails-application-on-ubuntu-14-04
 * Upgrading https://dev.to/jkostolansky/how-to-upgrade-postgresql-from-11-to-12-2la6

## Step 3: enable pg_stat_statements

```sql
ALTER SYSTEM SET shared_preload_libraries TO 'pg_stat_statements';
# service postgresql restart
ALTER SYSTEM SET pg_stat_statements.track TO 'all';
ALTER SYSTEM SET pg_stat_statements.max TO 10000;
ALTER SYSTEM SET track_activity_query_size TO 2048;
SELECT pg_reload_conf();
```

## Replication

 * https://31337.it/postgres%20replication/
 * https://valehagayev.wordpress.com/2018/08/15/postgresql-11-streaming-replication-hot-standby/
 * https://github.com/lesovsky/ansible-postgresql-sr-on-el6
 * https://info.crunchydata.com/blog/wheres-my-replica-troubleshooting-streaming-replication-synchronization-in-postgresql
 * https://www.percona.com/blog/2018/11/30/postgresql-streaming-physical-replication-with-slots/
 * https://www.percona.com/blog/2019/10/11/how-to-set-up-streaming-replication-in-postgresql-12/
 * https://www.2ndquadrant.com/en/blog/replication-configuration-changes-in-postgresql-12/

## Backup

 * https://www.fusionbox.com/blog/detail/postgresql-wal-archiving-with-wal-g-and-s3-complete-walkthrough/644/
 
