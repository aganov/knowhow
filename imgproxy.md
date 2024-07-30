```
apt install podman caddy
adduser deploy --disabled-password
loginctl enable-linger deploy
ufw allow 22/tcp
ufw allow 443
ufw enable
```

```
mkdir -p /etc/containers/systemd/users/1000/
vim /etc/containers/systemd/users/1000/imgproxy.container
chown -R deploy:deploy /etc/containers/systemd/users/1000/
```

```
[Unit]
Description=Imgproxy Container

[Container]
Image=docker.io/darthsim/imgproxy:latest
AutoUpdate=registry
Environment=AWS_ACCESS_KEY_ID=xxx
Environment=AWS_SECRET_ACCESS_KEY=xxx
Environment=IMGPROXY_KEY=xxx
Environment=IMGPROXY_SALT=xxx
Environment=IMGPROXY_USE_ETAG=true
Environment=IMGPROXY_USE_S3=true
Environment=IMGPROXY_S3_ENDPOINT=xxx
Environment=IMGPROXY_S3_REGION=xxx
PublishPort=8080:8080

[Install]
WantedBy=default.target
```

```
# vim /etc/caddy/Caddyfile
imgproxy.example.com {
  tls /etc/caddy/certificates/example.com.pem /etc/caddy/certificates/example.com.key
  reverse_proxy localhost:8080
}
```

```
systemctl --user -M deploy@ daemon-reload
systemctl --user -M deploy@ start imgproxy
systemctl --user -M deploy@ status imgproxy
systemctl restart caddy.service
```
