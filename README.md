# docker-server
Docker Server Nginx - PHP73 - PHP 74

## Install

- Install php 7.3 from fpm
  
  ```
  cd to/root
  docker build - < Dockerfile.php\:fpm73 -t php:fpm_73
  ```
  
- Install php 7.4 from fpm
  
  ```
  cd to/root
  docker build - < Dockerfile.php\:fpm74 -t php:fpm_74
  ```
  
  
- Install nginx from alpine
  
  ```
  cd to/root
  docker build - < Dockerfile.nginx\:alpine -t nginx:alpine
  ```
  
## FOR USE

```
cd to/root
docker-compose up -d
```

## file location

File location for read project located in:

/var/

## nginx setup

File location for nginx setup located in:

/nginx/conf.d/default.conf

After config this. Restart server or can use this on root file:

```
docker-compose restart
```

## example nginx setup

### Bono

```
  location /sim-5 {       
        location = /sim-5/ {
            rewrite ^(.*)$ /sim-5/index.php last;
        }

        alias  /var/www/sim-5/;

        if (-f $request_filename) {
            break;
        }

        rewrite ^(.*)$ /sim-5/index.php last;

        location ~ /sim-5/.+\.php$ {
            root           /var/www/sim-5/www;
            fastcgi_split_path_info ^(.+\.php)(/.+)$;
            fastcgi_pass   fpm73:9000;
            fastcgi_index  index.php;
            fastcgi_param  SCRIPT_FILENAME   /var/www/sim-5/www/index.php;
            include        fastcgi_params;
        }
    }
```

### Laravel

```
  location /checklist-prambanan {
        alias /var/www/checklist-prambanan/public;
        try_files $uri $uri/ @checklistPrambanan;

        location ~ \.php$ {
            include fastcgi_params;
            fastcgi_param SCRIPT_FILENAME $request_filename;
            fastcgi_pass fpm74:9000;
        }
    }

    location @checklistPrambanan {
        rewrite /checklist-prambanan/(.*)$ /checklist-prambanan/index.php?/$1 last;
    }
```