# Minio

## Install minio

Download the Minio server's binary file:

```sh
curl -O https://dl.minio.io/server/minio/release/linux-amd64/minio
chmod +x minio
mv minio /usr/local/bin
```

For security reasons, we don't want to run the Minio server as root.

```sh
useradd -r minio -s /sbin/nologin
chown minio:minio /usr/local/bin/minio
```

Next, we need to create a directory where Minio will store files. This will be the storage location for the buckets you'll create.

```sh
mkdir /var/www/s3.example.com
chown minio:minio /var/www/s3.example.com
```

The `/etc` directory is the most common location for server configuration files, so we'll create a place for Minio there.

```sh
mkdir /etc/minio
chown minio:minio /etc/minio
```

```sh
vim nano /etc/default/minio
```

```
MINIO_VOLUMES="/var/www/s3.example.com"
MINIO_OPTS="-C /etc/minio --address 127.0.0.1:9000"
```

## Installing the Minio Systemd Startup Script

```sh
vim /etc/systemd/system/minio.service
```

```
[Unit]
Description=Minio
Documentation=https://docs.minio.io
Wants=network-online.target
After=network-online.target
AssertFileIsExecutable=/usr/local/bin/minio

[Service]
WorkingDirectory=/usr/local/

User=minio
Group=minio

PermissionsStartOnly=true

EnvironmentFile=-/etc/default/minio
ExecStartPre=/bin/bash -c "[ -n \"${MINIO_VOLUMES}\" ] || echo \"Variable MINIO_VOLUMES not set in /etc/default/minio\""

ExecStart=/usr/local/bin/minio server $MINIO_OPTS $MINIO_VOLUMES

# Let systemd restart this service only if it has ended with the clean exit code or signal.
Restart=on-success

StandardOutput=journal
StandardError=inherit

# Specifies the maximum file descriptor number that can be opened by this process
LimitNOFILE=65536

# Disable timeout logic and wait until process is stopped
TimeoutStopSec=0

# SIGTERM signal is used to stop Minio
KillSignal=SIGTERM

SendSIGKILL=no

SuccessExitStatus=0

[Install]
WantedBy=multi-user.target

# Built for ${project.name}-${project.version} (${project.name})
```

```sh
systemctl daemon-reload
systemctl enable minio
systemctl start minio
systemctl status minio
```

You should get output like the following:

```
● minio.service - Minio
   Loaded: loaded (/etc/systemd/system/minio.service; enabled; vendor preset: enabled)
   Active: active (running) since Sun 2018-03-18 19:15:19 UTC; 3 weeks 3 days ago
     Docs: https://docs.minio.io
 Main PID: 3266 (minio)
   CGroup: /system.slice/minio.service
           └─3266 /usr/local/bin/minio server -C /etc/minio --address 127.0.0.1:9000 /var/www/s3.example.com
```

## Setup nginx proxy

Letsencrypt is used to for ssl certificate

```sh
vim /etc/nginx/sites-enabled/s3.example.com
```

```
server {
  listen 80;
  listen 443 ssl;
  server_name s3.example.com;

  location /.well-known {
    alias /var/www/s3.example.com/.well-known;
  }

  location / {
    proxy_set_header Host $http_host;
    proxy_pass http://localhost:9000;
  }

  ssl_dhparam          /etc/nginx/ssl/dhparams.pem;
  ssl_certificate      /etc/letsencrypt/live/s3.example.com/fullchain.pem;
  ssl_certificate_key  /etc/letsencrypt/live/s3.example.com/privkey.pem;
}
```

```sh
service nginx reload
```
