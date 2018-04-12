# Deploy user

```sh
adduser deploy --disabled-password
ssh-copy-id -i ~/.ssh/id_rsa.pub root@example.com # on local machine
mkdir /home/deploy/.ssh
cp /root/.ssh/authorized_keys /home/deploy/.ssh
chown deploy:deploy /home/deploy/.ssh -R
chmod 600 /home/deploy/.ssh/authorized_keys
```