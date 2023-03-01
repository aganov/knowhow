# Node.js

https://github.com/nodesource/distributions#installation-instructions

https://yarnpkg.com/en/docs/install

```bash
apt-get install git aptitude apt-transport-https

curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | sudo apt-key add -
echo "deb https://dl.yarnpkg.com/debian/ stable main" | sudo tee /etc/apt/sources.list.d/yarn.list
curl -sL https://deb.nodesource.com/setup_18.x | sudo -E bash -
sudo apt-get install -y nodejs yarn
```
