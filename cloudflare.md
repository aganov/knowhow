```
vim /etc/cron.weekly/update-cfips
```

```
#!/bin/bash

set -e

cf_ips() {
echo "# https://www.cloudflare.com/ips"

for type in v4 v6; do
echo "# IP$type"
curl -sL "https://www.cloudflare.com/ips-$type/" | sed "s|^|allow |g" | sed "s|\$|;|g"
echo
done

echo "# Generated at $(LC_ALL=C date)"
}

(cf_ips && echo "deny all; # deny all remaining ips") > /etc/nginx/allow-cloudflare-only.conf

# reload Nginx
# sudo systemctl reload nginx
```

```
chmod +x /etc/cron.weekly/update-cfips
/etc/cron.weekly/update-cfips
```

```
server {
  listen 80;
  listen [::]:80;
  server_name example.com;

  include /etc/nginx/allow-cloudflare-only.conf;

  # the rest of config here…
}
```
