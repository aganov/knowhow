Based on https://github.com/mperham/sidekiq/blob/master/examples/systemd/sidekiq.service

1) Create `/etc/systemd/system/sidekiq.service`

```
[Unit]
Description=sidekiq
After=syslog.target network.target

[Service]
Type=simple
WorkingDirectory=/var/www/example.com/current
ExecStart=/bin/bash -lc '/usr/local/rbenv/shims/bundle exec sidekiq -e production'

# use `systemctl reload sidekiq` to send the quiet signal to Sidekiq
# at the start of your deploy process.
ExecReload=/usr/bin/kill -TSTP $MAINPID

User=deploy
Group=deploy
UMask=0002

# Greatly reduce Ruby memory fragmentation and heap usage
# https://www.mikeperham.com/2018/04/25/taming-rails-memory-bloat/
# Environment=MALLOC_ARENA_MAX=2

RestartSec=1
Restart=on-failure

StandardOutput=syslog
StandardError=syslog

# This will default to "bundler" if we don't specify it
SyslogIdentifier=sidekiq

[Install]
WantedBy=multi-user.target
```

2) `systemctl enable sidekiq`

3) Use `visudo` and allow `deploy` user to restart and reload sidekiq

```
%deploy ALL= NOPASSWD: /bin/systemctl restart sidekiq
%deploy ALL= NOPASSWD: /bin/systemctl reload sidekiq
```

4) Add some tasks to capistrano

```ruby
set :sidekiq_service_name, "sidekiq"

namespace :sidekiq do
  desc "Quiet sidekiq (stop fetching new tasks from Redis)"
  task :quiet do
    on roles(:app) do
      execute :sudo, :systemctl, :reload, fetch(:sidekiq_service_name)
    end
  end

  desc "Restart sidekiq service"
  task :restart do
    on roles(:app) do
      execute :sudo, :systemctl, :restart, fetch(:sidekiq_service_name)
    end
  end
end

after "deploy:starting", "sidekiq:quiet"
after "deploy:published", "sidekiq:restart"
```

5) You can use `systemctl {start,stop,restart,reload} sidekiq`
