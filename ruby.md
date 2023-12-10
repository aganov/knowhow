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
apt-get install autoconf patch build-essential rustc libssl-dev libyaml-dev libreadline6-dev zlib1g-dev libgmp-dev libncurses5-dev libffi-dev libgdbm6 libgdbm-dev libdb-dev uuid-dev
apt-get install libjemalloc-dev # If you plan to install ruby `--with-jemalloc`
```

Prefix `rbenv install` with `RUBY_CONFIGURE_OPTS=--with-jemalloc` to install ruby with jemalloc instead of `glibc malloc`. This is known to cause problems with passenger in the past. @see https://github.com/phusion/passenger/issues/1747


```bash
RUBY_CONFIGURE_OPTS="--enable-yjit --with-jemalloc" rbenv install 3.2.2
rbenv global 3.2.2
gem update --system
ruby --version
```

```bash
echo 'gem: --no-document' | tee /root/.gemrc
echo 'gem: --no-document' | tee /home/deploy/.gemrc
chown deploy:deploy /home/deploy/.gemrc

gem install bundler
```
