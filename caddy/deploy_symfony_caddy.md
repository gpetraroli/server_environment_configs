[Index](../README.md) > Deploy a Symfony application in a Caddy enviroment

# Deploy a Symfony application in a Caddy enviroment

If folder /var/www does not exists create it:
```
sudo mkdir /var/www
```

Change the ownership of this folder to the current user and group:
```
sudo chown -R $USER:$USER /var/www
```

Clone project into /var/www folder:
```
cd /var/www
sudo git clone <git_repository_url>
cd <project_name>
```

Install composer dependencies:
```
composer Install
```

Modify the .env file as required and generate an optimized .env.local.php
```
composer dump-env prod
```
> **NOTE:** If you make any change to the .env file in the future don't forget to run this command again.

Change the project's var/log and var/cache directory permissions:
```
chmod -R 777 ./var/log
chmod -R 777 ./var/cache
```
> **NOTE:** This is not the best way to change permissions; the most safe and recommended way is by using ACL, see [Symfony documentation](https://symfony.com/doc/current/setup/file_permissions.html#configuring-permissions-for-symfony-applications)

Install npm dependencies and build assets:
```
npm install
npm run build
```

Create the database and run migrations
```
php bin/console d:d:c
php bin/console d:m:m
```

===========================================================================
>> Now you just need to configure your CaddyFile before enjoy your Symfony application!
>>### Next: [Configure the Caddyfile](./caddyfile_symfony.md)
>>### Back to the index : [Index](../README.md)

