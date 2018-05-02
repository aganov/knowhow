# rbenv

```bash
git clone https://github.com/sstephenson/rbenv.git /usr/local/rbenv
vim /etc/profile.d/rbenv.sh
```

```bash
# rbenv setup
export RBENV_ROOT=/usr/local/rbenv
export PATH="$RBENV_ROOT/bin:$PATH"
eval "$(rbenv init -)"
```

> Save and exit :wq! (Shift + ZZ)


```bash
chmod +x /etc/profile.d/rbenv.sh
```

> Exit and login again to load rbenv

# Install ruby

## Install latest ruby-build

``` bash
mkdir /usr/local/rbenv/plugins
git clone https://github.com/sstephenson/ruby-build.git /usr/local/rbenv/plugins/ruby-build
```

## Install latest stable ruby

 * Install system dependenciees first https://github.com/rbenv/ruby-build/wiki#suggested-build-environment
 * Install some other stuff

```bash
aptitude install autoconf bison build-essential libssl-dev libyaml-dev libreadline6-dev zlib1g-dev libncurses5-dev libffi-dev libgdbm3 libgdbm-dev
aptitude install libcurl4-openssl-dev libpcre3-dev libxml2 libxml2-dev libxslt1-dev
```

```bash
rbenv install 2.4.3
rbenv global 2.4.3
ruby --version
```

```bash
echo 'gem: --no-document' > /root/.gemrc
echo 'gem: --no-document' > /home/deploy/.gemrc
chown deploy:deploy /home/deploy/.gemrc

gem install bundler
```
