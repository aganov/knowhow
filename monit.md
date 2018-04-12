# Monit

## Configure httpd server

First we need to install monit

```sh
aptitude install monit

```

Then edit `monitrc` file and uncomment the `httpd` section

```
vim /etc/monit/monitrc

```

```
set httpd port 2812 and
    use address localhost  # only accept connection from localhost
    allow localhost        # allow localhost to connect to the server and
    allow admin:monit      # require user 'admin' with password 'monit'
```

Restart monit to apply changes


```bash
service monit restart
monit summary
```



## Add monit to sudoers

```bash
visudo
deploy  ALL=NOPASSWD:/usr/bin/monit
```