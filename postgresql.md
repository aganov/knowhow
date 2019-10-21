# PostgreSQL on Ubuntu 18.04 (with APT)

NOTICE: Use https://www.postgresql.org/download/linux/ubuntu/ to find proper installation instructions

## Step 1: install postgressql-12.xx

```bash
wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -
sh -c 'echo deb http://apt.postgresql.org/pub/repos/apt/ bionic-pgdg main > /etc/apt/sources.list.d/pgdg.list'
apt-get update
apt-get install postgresql-10 libpq-dev
```

## Step 2: tune PostgreSQL

 * Visit: http://pgtune.leopard.in.ua/ and enter Parameters of your system
 * Modify `/etc/postgresql/12/main/postgresql.conf` according values from pgtune
 * `service postgresql restart`
 * Create Database User https://www.digitalocean.com/community/tutorials/how-to-use-postgresql-with-your-ruby-on-rails-application-on-ubuntu-14-04
 * Upgrading https://medium.com/@tk512/upgrading-postgresql-from-9-4-to-9-5-on-ubuntu-14-04-lts-dfd93773d4a5#d685
 * Upgrading https://dev.to/jkostolansky/how-to-upgrade-postgresql-from-11-to-12-2la6

## Replication

 * https://31337.it/postgres%20replication/
 * https://valehagayev.wordpress.com/2018/08/15/postgresql-11-streaming-replication-hot-standby/
 * https://github.com/lesovsky/ansible-postgresql-sr-on-el6
 * https://info.crunchydata.com/blog/wheres-my-replica-troubleshooting-streaming-replication-synchronization-in-postgresql
 * https://www.percona.com/blog/2018/11/30/postgresql-streaming-physical-replication-with-slots/
 * https://www.percona.com/blog/2019/10/11/how-to-set-up-streaming-replication-in-postgresql-12/
 * https://www.2ndquadrant.com/en/blog/replication-configuration-changes-in-postgresql-12/
