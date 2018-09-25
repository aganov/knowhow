
```sh
locale-gen en_US.UTF-8
```

Insert into /etc/default/locale:

```
LC_CTYPE="en_US.UTF-8"
LC_ALL="en_US.UTF-8"
LANG="en_US.UTF-8"
```

```sh
source /etc/default/locale
```
