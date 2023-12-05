[Index](../README.md) > Configure a VPS with Caddy, PHP, MySQL

# Configure a VPS with Caddy, PHP, MySQL

This section will guide you through the installation of the following packages:
- Ubuntu 22.04
- Caddy
- PHP 8.2
- MySQL (latest)
- Composer (latest)
- Node
- Git

============================================================================

## Install Caddy

Installation doc:
https://caddyserver.com/docs/install#debian-ubuntu-raspbian

```
sudo apt install -y debian-keyring debian-archive-keyring apt-transport-https
curl -1sLf 'https://dl.cloudsmith.io/public/caddy/stable/gpg.key' | sudo gpg --dearmor -o /usr/share/keyrings/caddy-stable-archive-keyring.gpg
curl -1sLf 'https://dl.cloudsmith.io/public/caddy/stable/debian.deb.txt' | sudo tee /etc/apt/sources.list.d/caddy-stable.list
sudo apt update
sudo apt install caddy
```

Check if Caddy is up and running:
>```sudo systemctl status caddy```

============================================================================

## PHP 8.2

Add 'Ondrej sury' repository that contains php2
```
sudo add-apt-repository ppa:ondrej/php
sudo apt update
```

Install PHP 8.2:
```
sudo apt install --no-install-recommends php8.2
```
> **Note:** The --no-install-recommends flag will ensure that other packages like the Apache web server are not installed.

Besides PHP itself, you will likely want to install some additional PHP modules. You can use this command to install additional modules, replacing PACKAGE_NAME with the package you wish to install:

```
sudo apt install php8.1-PACKAGE_NAME
```

To enable PHP support for Caddy, you need to install PHP-FPM:
```
sudo apt install php8.2-fpm
```

Here are a few suggestions of the most common modules you will most likely want to install:

```
sudo apt install -y php8.2-cli php8.2-common php8.2-mysql php8.2-zip php8.2-gd php8.2-mbstring php8.2-curl php8.2-xml php8.2-bcmath php-json php8.2-intl
```

If you have projects based on Sqlite:
```
sudo apt install php8.2-sqlite3
```

============================================================================

## MySQL

### Install the mysql-server package
```
sudo apt install mysql-server
```

Ensure that the server is running using the systemctl start command:
```
sudo systemctl start mysql.service
```

### Configure mysql

First, open up the MySQL prompt:
```
sudo mysql
```

Then run the following ALTER USER command to change the root user’s authentication method to one that uses a password. The following example changes the authentication method to mysql_native_password:
```
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'password';
```
> **!** Make sure to change the password 'password' with a stronger one!

After making this change, exit the MySQL prompt:
```
exit
```

Run the security script with sudo:
```
sudo mysql_secure_installation
```

Once the security script completes, you can then reopen MySQL and change the root user’s authentication method back to the default, auth_socket.<br>
To do so first authenticate as the root MySQL user using a password:
```
mysql -u root -p
```

Then go back using the default authentication method:
```
ALTER USER 'root'@'localhost' IDENTIFIED WITH auth_socket;
```

You can now exit:
```
exit
```

============================================================================

## Composer

Run:
```
cd ~
curl -sS https://getcomposer.org/installer -o /tmp/composer-setup.php
```

```
HASH=`curl -sS https://composer.github.io/installer.sig`
```

```
php -r "if (hash_file('SHA384', '/tmp/composer-setup.php') === '$HASH') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
```

if output is 'Installer verified' you can continue

```
sudo php /tmp/composer-setup.php --install-dir=/usr/local/bin --filename=composer
```

============================================================================

## Node

Install NodeJS:
```
sudo apt install nodejs
```

Install npm:
```
sudo apt install npm
```

Update NodeJS to the latest version using nvm:<br>
Install nvm:
```
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.5/install.sh | bash
```

Restart bash:
```
source ~/.bashrc
```

Now update node:
```
nvm install node
```

===========================================================================

## Git

normaly already installed, if not:

```
sudo apt install git
```

### Add a new ssh key

```
ssh-keygen -t ed25519 -C "your_email@example.com"
```
> **!** Make sure to to change the email with yours

===========================================================================
>> Now you have a VPS with Caddy, PHP, MySQL, Composer, Node and Git installed and configured. Next step is to deploy a Symfony application in this enviroment.
>> ### Next: [Deploy a Symfony application in a Caddy enviroment](./deploy_symfony_caddy.md)
>> ### Back to the index : [Index](../README.md)
