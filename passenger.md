# Passenger + Nginx on Ubuntu 18.04 LTS (with APT)

NOTICE: Use https://www.phusionpassenger.com/library/install/nginx/install/oss/ to find proper setup instructions

NOTICE: Find a way to add https://github.com/openresty/headers-more-nginx-module

## Step 1: install nginx

```sh
wget -O nginx_signing.key http://nginx.org/keys/nginx_signing.key
sudo apt-key add nginx_signing.key
echo "deb http://nginx.org/packages/ubuntu/ bionic nginx
deb-src http://nginx.org/packages/ubuntu/ bionic nginx" >> /etc/apt/sources.list.d/nginx.list
apt-get update
apt-get install nginx-extras
```

## Step 2: install Passenger packages

```bash
# Install our PGP key and add HTTPS support for APT
sudo apt-get install -y dirmngr gnupg
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 561F9B9CAC40B2F7
sudo apt-get install -y apt-transport-https ca-certificates

# Add our APT repository
sudo sh -c 'echo deb https://oss-binaries.phusionpassenger.com/apt/passenger bionic main > /etc/apt/sources.list.d/passenger.list'
sudo apt-get update

# Install Passenger + Nginx
sudo apt-get install -y libnginx-mod-http-passenger
```

## Step 3: enable the Passenger Nginx module and restart Nginx

```bash
if [ ! -f /etc/nginx/modules-enabled/50-mod-http-passenger.conf ]; then sudo ln -s /usr/share/nginx/modules-available/mod-http-passenger.load /etc/nginx/modules-enabled/50-mod-http-passenger.conf ; fi
sudo ls /etc/nginx/conf.d/mod-http-passenger.conf
```

## Step 4: check installation

```bash
passenger-config validate-install
passenger-memory-stats
```

## Step 5: setup SSL (Optional)

NOTICE:
 * https://weakdh.org/sysadmin.html
 * https://certbot.eff.org/

```
# Enable Diffie-Hellman for TLS
mkdir /etc/nginx/ssl
openssl dhparam -out /etc/nginx/ssl/dhparams.pem 2048
```

`/etc/nginx/nginx.conf`

```
user deploy;
worker_processes auto;
pid /run/nginx.pid;
include /etc/nginx/modules-enabled/*.conf;

events {
  worker_connections 768;
  # multi_accept on;
}

http {
  include       mime.types;
  default_type  application/octet-stream;

  access_log /var/log/nginx/access.log;
  error_log /var/log/nginx/error.log;

  # server_names_hash_bucket_size 64;
  # server_name_in_redirect off;

  ssl_ciphers 'ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-AES256-GCM-SHA384:DHE-RSA-AES128-GCM-SHA256:DHE-DSS-AES128-GCM-SHA256:kEDH+AESGCM:ECDHE-RSA-AES128-SHA256:ECDHE-ECDSA-AES128-SHA256:ECDHE-RSA-AES128-SHA:ECDHE-ECDSA-AES128-SHA:ECDHE-RSA-AES256-SHA384:ECDHE-ECDSA-AES256-SHA384:ECDHE-RSA-AES256-SHA:ECDHE-ECDSA-AES256-SHA:DHE-RSA-AES128-SHA256:DHE-RSA-AES128-SHA:DHE-DSS-AES128-SHA256:DHE-RSA-AES256-SHA256:DHE-DSS-AES256-SHA:DHE-RSA-AES256-SHA:AES128-GCM-SHA256:AES256-GCM-SHA384:AES128-SHA256:AES256-SHA256:AES128-SHA:AES256-SHA:AES:CAMELLIA:DES-CBC3-SHA:!aNULL:!eNULL:!EXPORT:!DES:!RC4:!MD5:!PSK:!aECDH:!EDH-DSS-DES-CBC3-SHA:!EDH-RSA-DES-CBC3-SHA:!KRB5-DES-CBC3-SHA';
  ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
  ssl_prefer_server_ciphers on;

  sendfile on;
  tcp_nopush on;
  tcp_nodelay on;
  keepalive_timeout 65;
  types_hash_max_size 2048;
  client_max_body_size 128m;
  server_tokens off;

  # server_names_hash_bucket_size 64;
  # server_name_in_redirect off

  gzip on;
  gzip_disable "msie6";

  gzip_vary on;
  gzip_proxied any;
  gzip_comp_level 6;
  gzip_buffers 16 8k;
  gzip_http_version 1.1;
  gzip_types text/plain text/css application/json application/x-javascript application/javascript text/xml application/xml application/xml+rss text/javascript;

  # passenger_pool_idle_time 0;
  more_clear_headers 'Server' 'X-Powered-By' 'X-Runtime';

  include /etc/nginx/conf.d/*.conf;
  include /etc/nginx/sites-enabled/*;
}
```

Create default site

`/etc/nginx/sites-enabled/example.com`

```
server {
  listen 80 default_server;
  listen 443 ssl;
  server_name example.com;
  access_log /dev/null;
  error_log /dev/null;

  passenger_enabled on;
  root /var/www/example.com/current/public;

  ssl_dhparam          /etc/nginx/ssl/dhparams.pem;
  ssl_certificate      /etc/nginx/ssl/example.com.pem;
  ssl_certificate_key  /etc/nginx/ssl/example.com.key;
}
```
