# php

This Dockerfile creates a useful php-fpm container.  It provides `/var/www` as a volume to mount, allowing you to mount your host's volume into this container for use with PHP scripts.

The default is to use UID and GID 33 for the www-data user, which is what Debian uses.  If you're running this on a Fedora / CentOS / RHEL or similar host, you'll need to declare UID and GID 48:
```
sudo docker build --build-arg UID=48 --build-arg GID=48 -t skpy:php .
```
