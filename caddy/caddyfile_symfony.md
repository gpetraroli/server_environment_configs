[Index](../README.md) > [Deploy a Symfony application in a Caddy enviroment](./deploy_symfony_caddy.md) > Caddyfile for a Symfony application

# Caddyfile for a Symfony application

Open the Caddyfile with a text editor:
```
sudo nano /etc/caddy/Caddyfile
```

Add this directives (change the hosts, the path to your project public directory, and your email):
```
old.antoine-dumez.com {
    root *  /var/www/my_web_cv/public/

    # decomment this line if you want to associate this email to the TLS certificate
    #tls your_email@email.com

    encode zstd gzip
    file_server

    php_fastcgi unix//var/run/php/php8.2-fpm.sock {
        resolve_root_symlink
    }

    @phpFile {
        path *.php*
    }
    error @phpFile "Not found" 404
}
```

> This is the minimum configuration need it. For more see the [official Caddy documentation](https://caddyserver.com/docs/)

Restart Caddy service:
```
sudo systemctl restart caddy
```

===========================================================================
>> Now you should have a working Symfony application in a Caddy enviroment!
>>### Back to the index : [Index](../README.md)