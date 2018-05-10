# Installation

Install either [Docker](https://www.docker.com/) or [Docker for Mac](https://www.docker.com/docker-mac)

# Setup

This Rails trickery allows us to not specify a database name in our DATABASE_URL environment variable, and instead use the config file to define it. The reason I like doing this is because it allows us run tests against the same DB without having to swizzle around DATABASE_URL values.

`config/databse.yml`

```yml
default: &default
  adapter: postgresql
  encoding: unicode
  pool: <%= ENV.fetch("RAILS_MAX_THREADS") { 5 } %>
  url: <%= ENV["DATABASE_URL"] %>

development:
  <<: *default
  database: app_development

test:
  <<: *default
  database: app_test
```

# Run

```sh
docker-compose up --build
docker-compose run --rm web rake db:create db:migrate db:seed
```

The `--rm` flag ensures the container that is used to run this command is removed afterwards. This saves space on your local machine.

# Debugging

To actually halt execution and use pry as normal, you have to define these options to docker-compose.yml:

```yaml
web:
  tty: true
  stdin_open: true
```

In order to attach to a docker container, you need to know what its ID is. Use `docker ps` to get a list of the running containers and their ids.

```
-> % docker ps
CONTAINER ID        IMAGE                COMMAND                  CREATED             STATUS                    PORTS                    NAMES
d47d07b5eb7f        app_webpacker        "bin/webpack-dev-ser…"   30 minutes ago      Up 30 minutes             0.0.0.0:3035->3035/tcp   app_webpacker_1
9cc89e0d8dc8        app_sidekiq          "sidekiq -C config/s…"   30 minutes ago      Up 30 minutes                                      app_sidekiq_1
af910e62d36b        app_web              "rails s"                30 minutes ago      Up 30 minutes             0.0.0.0:3000->3000/tcp   app_web_1
812ba7a7ba2e        minio/minio          "/usr/bin/docker-ent…"   About an hour ago   Up 30 minutes (healthy)   0.0.0.0:9000->9000/tcp   app_minio_1
87f489b6e3da        redis:3.2-alpine     "docker-entrypoint.s…"   About an hour ago   Up 30 minutes             0.0.0.0:6379->6379/tcp   app_redis_1
1a0f1f99c245        postgres:10-alpine   "docker-entrypoint.s…"   About an hour ago   Up 30 minutes             0.0.0.0:5432->5432/tcp   app_postgres_1
```

Then you can use the numeric ID to attach to the docker instance:


```sh
docker attach af910e62d36b
```

If you keep this attached, it should show the rails console prompt the next time you hit the `pry breakpoint`.

# Security

NB: If you are using this for staging/production deployment don't forget to disallow all incomming traffic except `22`,  `80` and `433` ports, using your cloud service firewall or `ufw`

# Cleanup

```sh
docker system prune -a --volumes
docker network prune
```
