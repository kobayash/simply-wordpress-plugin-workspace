# simply-wordpress-plugin-workspace
Simply workspace for WordPress plugin development.

# How to use

## WordPress(Latest) Download
```
$ docker exec -it -u docker wordpress wp core download --locale=ja --force
```

## Create wp-config.php
```
$ docker exec -it -u docker wordpress wp core config --dbname=wordpress --dbuser=root --dbhost=mysql --dbpass=root
```

## Create database
```
$ docker exec -it -u docker wordpress wp db create
```

## WordPress Install
```
$ docker exec -it -u docker wordpress wp core install --url=http://localhost --title=WordPress --admin_user=root --admin_email=root@example.com --skip-email --admin_password=root
```

## Link Plugin to Volume
```
$ docker exec -it -u docker wordpress ln -s /plugin ./wp-content/plugins/MyPlugin
```

## Scaffold Plugin

```
$ docker exec -it -u docker wordpress wp scaffold plugin MyPlugin --ci=circle
```

## Install Test Environment
```
$ docker exec -it -u docker wordpress ./wp-content/plugins/MyPlugin/bin/install-wp-tests.sh wordpress_test root root mysql
```

## Execute Test via PHPUnit
```
$ docker exec -it -u docker wordpress phpunit --configuration wp-content/plugins/MyPlugin/
```
