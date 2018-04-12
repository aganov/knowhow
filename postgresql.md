# PostgreSQL on Ubuntu 16.04 (with APT)

NOTICE: Use https://www.postgresql.org/download/linux/ubuntu/ to find proper installation instructions

## Step 1: install postgressql-9.xx

```bash
wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -
sh -c 'echo deb http://apt.postgresql.org/pub/repos/apt/ xenial-pgdg main > /etc/apt/sources.list.d/pgdg.list'
aptitude update
apt-get install postgresql-9.6 libpq-dev
```

## Step 2: tune PostgreSQL

 * Visit: http://pgtune.leopard.in.ua/ and enter Parameters of your system
 * Modify `/etc/postgresql/9.6/main/postgresql.conf` according values from pgtune
 * `service postgresql restart`
 * Create Database User https://www.digitalocean.com/community/tutorials/how-to-use-postgresql-with-your-ruby-on-rails-application-on-ubuntu-14-04
 * Upgrading https://medium.com/@tk512/upgrading-postgresql-from-9-4-to-9-5-on-ubuntu-14-04-lts-dfd93773d4a5#d685
