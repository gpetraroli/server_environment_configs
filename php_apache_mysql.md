============================================================================

## Apache2

https://ubuntu.com/tutorials/install-and-configure-apache#1-overview

```
sudo apt update
sudo apt install apache2
```

Activate the rewrite mod, this is need it for symfony applications configs: without the url will contains /index.php at the end

```
sudo a2enmod rewrite
```

### Adding Rewrite Rules

(to do after all other installations)

https://symfony.com/doc/current/setup/web_server_configuration.html#adding-rewrite-rules

============================================================================

## PHP 8.1

https://www.digitalocean.com/community/tutorials/how-to-install-php-8-1-and-set-up-a-local-development-environment-on-ubuntu-22-04
Install php: (The --no-install-recommends flag will ensure that other packages like the Apache web server are not installed.)

```
sudo apt install --no-install-recommends php8.1
```

Besides PHP itself, you will likely want to install some additional PHP modules. You can use this command to install additional modules, replacing PACKAGE_NAME with the package you wish to install:

```
sudo apt-get install php8.1-PACKAGE_NAME
```

Here are a few suggestions of the most common modules you will most likely want to install:

```
sudo apt-get install -y php8.1-cli php8.1-common php8.1-mysql php8.1-zip php8.1-gd php8.1-mbstring php8.1-curl php8.1-xml php8.1-bcmath php-json
```

============================================================================

## MySQL

https://www.digitalocean.com/community/tutorials/how-to-install-mysql-on-ubuntu-20-04

Install the mysql-server package:

```
sudo apt install mysql-server
```

Ensure that the server is running using the systemctl start command:

```
sudo systemctl start mysql.service
```

### Configuration

First, open up the MySQL prompt:

```
sudo mysql
```

Then run the following ALTER USER command to change the root user’s authentication method to one that uses a password. The following example changes the authentication method to mysql_native_password:

```
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'password';
```

After making this change, exit the MySQL prompt:

```
mysql> exit
```

Run the security script with sudo:

```
sudo mysql_secure_installation
```

Once the security script completes, you can then reopen MySQL and change the root user’s authentication method back to the default, auth_socket. To authenticate as the root MySQL user using a password, run this command:

```
mysql -u root -p
```

Then go back to using the default authentication method using this command:

```
ALTER USER 'root'@'localhost' IDENTIFIED WITH auth_socket;
```

============================================================================

## phpMyAdmin

https://www.digitalocean.com/community/tutorials/how-to-install-and-secure-phpmyadmin-on-ubuntu-20-04

```
sudo apt install phpmyadmin
```

The installation process adds the phpMyAdmin Apache configuration file into the /etc/apache2/conf-enabled/ directory, where it is read automatically. To finish configuring Apache and PHP to work with phpMyAdmin, the only remaining task in this section of the tutorial is to is explicitly enable the mbstring PHP extension, which you can do by typing:

```
sudo phpenmod mbstring
```

Afterwards, restart Apache for your changes to be recognized:

```
sudo systemctl restart apache2
```

phpMyAdmin is now installed and configured to work with Apache. However, before you can log in and begin interacting with your MySQL databases, you will need to ensure that your MySQL users have the privileges required for interacting with the program.

In order to log in to phpMyAdmin as your root MySQL user, you will need to switch its authentication method from auth_socket to one that makes use of a password, if you haven’t already done so. To do this, open up the MySQL prompt from your terminal:

```
sudo mysql
```

```
ALTER USER 'root'@'localhost' IDENTIFIED WITH caching_sha2_password BY 'password';
```

### Securing phpMyAdmin Instance

Because of its ubiquity, phpMyAdmin is a popular target for attackers, and you should take extra care to prevent unauthorized access. One way of doing this is to place a gateway in front of the entire application by using Apache’s built-in .htaccess authentication and authorization functionalities.

```
sudo nano /etc/apache2/conf-available/phpmyadmin.conf
```

Add an AllowOverride All directive within the <Directory /usr/share/phpmyadmin> section of the configuration file, like this.

To implement the changes you made, restart Apache:

```
sudo systemctl restart apache2
```

Now that you have enabled the use of .htaccess files for your application, you need to create one to actually implement some security.

In order for this to be successful, the file must be created within the application directory. You can create the necessary file and open it in your text editor with root privileges by typing:

```
sudo nano /usr/share/phpmyadmin/.htaccess
```

Within this file, enter the following information:

```
AuthType Basic
AuthName "Restricted Files"
AuthUserFile /etc/phpmyadmin/.htpasswd
Require valid-user
```

The location that you selected for your password file was /etc/phpmyadmin/.htpasswd. You can now create this file and pass it an initial user with the htpasswd utility:

```
sudo htpasswd -c /etc/phpmyadmin/.htpasswd <username>
```

Then restart Apache to put .htaccess authentication into effect:

```
sudo systemctl restart apache2
```

Now, when you access your phpMyAdmin subdirectory, you will be prompted for the additional account name and password that you just configured:

```
https://domain_name_or_IP/phpmyadmin
```

============================================================================

## Composer

https://www.digitalocean.com/community/tutorials/how-to-install-and-use-composer-on-ubuntu-20-04

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

https://www.digitalocean.com/community/tutorials/how-to-install-node-js-on-ubuntu-20-04

```
sudo apt install nodejs
```

```
sudo apt install npm
```

===========================================================================

## Git

https://www.digitalocean.com/community/tutorials/how-to-install-git-on-ubuntu-20-04

normaly already installed, if not:

```
sudo apt install git
```

### Add a new ssh key

```
ssh-keygen -t ed25519 -C "your_email@example.com"
```
