# UFW - program for managing a netfilter firewall

First, obviously, you want to make sure UFW is installed. It should be installed by default in Ubuntu, but if for some reason it’s not, you can install the package using aptitude or apt-get using the following commands:

```
apt install ufw
```

## Check the Status

```
ufw status
```

## Set Up Defaults

One of the things that will make setting up any firewall easier is to define some default rules for allowing and denying connections. UFW’s defaults are to deny all incoming connections and allow all outgoing connections. This means anyone trying to reach your cloud server would not be able to connect, while any application within the server would be able to reach the outside world. To set the defaults used by UFW, you would use the following commands:

```
ufw default deny incoming
ufw default allow outgoing
```

## Allow Connections

The syntax is pretty simple. You change the firewall rules by issuing commands in the terminal. If we turned on our firewall now, it would deny all incoming connections. If you’re connected over SSH to your cloud server, that would be a problem because you would be locked out of your server. Let’s enable SSH connections to our server to prevent that from happening:

```
sudo ufw allow ssh
```

As you can see, the syntax for adding services is pretty simple. UFW comes with some defaults for common uses. Our SSH command above is one example. It’s basically just shorthand for:

The command below will add the rule for the PostgreSQL default port, which is 5432. If you've changed that port, be sure to update it in the command below. Make sure that you've used the IP address of the server that needs access. If need be, re-run this command to add each client IP address that needs access:

```
ufw allow from $CLIENT_IP_ADDRESS to any port 5432
```

and for Redis

```
ufw allow from $CLIENT_IP_ADDRESS to any port 6379
```

## Allow All Incoming HTTP

```
ufw allow http
```

Allow All Incoming HTTPS

```
ufw allow https
```

## Check added rules

```
ufw show added
```

```
Added user rules (see 'ufw status' for running firewall):
ufw allow 22/tcp
ufw allow 80/tcp
ufw allow 443/tcp
```

## Turn It On

After we’ve gotten UFW to where we want it, we can turn it on using this command (remember: if you’re connecting via SSH, make sure you’ve set your SSH port, commonly port 22, to be allowed to receive connections):


```
ufw enable
```

You should see the command prompt again if it all went well. You can check the status of your rules now by typing:

```
sudo ufw status
```

or

```
ufw status verbose
```

```
Status: active
Logging: on (low)
Default: deny (incoming), allow (outgoing), disabled (routed)
New profiles: skip

To                         Action      From
--                         ------      ----
22/tcp                     ALLOW IN    Anywhere
80/tcp                     ALLOW IN    Anywhere
443/tcp                    ALLOW IN    Anywhere
22/tcp (v6)                ALLOW IN    Anywhere (v6)
80/tcp (v6)                ALLOW IN    Anywhere (v6)
443/tcp (v6)               ALLOW IN    Anywhere (v6)
```

To turn UFW off, use the following command:

```
ufw disable
```

## Reset Everything

If, for whatever reason, you need to reset your cloud server’s rules to their default settings, you can do this by typing this command:

```
ufw reset
```
