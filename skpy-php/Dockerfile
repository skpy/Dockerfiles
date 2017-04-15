FROM alpine:latest

# ensure www-data user exists with uid 48 for use on Fedora
RUN set -xe \
  delgroup xfs \
  && deluser xfs \
  && addgroup -g 33 -S www-data \
  && adduser -u 33 -D -S -G www-data www-data

RUN apk add --no-cache \
  php7 php7-fpm php7-curl php7-dom php7-gd php7-iconv php7-json \
  php7-mbstring php7-mcrypt php7-mysqli \
  php7-pcntl php7-pdo php7-posix php7-session \
  ssmtp

# set desired PHP settings
RUN set -ex \
  && { \
    echo '[global]'; \
    echo 'error_log = /proc/self/fd/2'; \
    echo; \
    echo '[www]'; \
    echo '; if we send this to /proc/self/fd/1, it never appears'; \
    echo 'access.log = /proc/self/fd/2'; \
    echo; \
    echo 'clear_env = no'; \
    echo; \
    echo '; Ensure worker stdout and stderr are sent to the main error log.'; \
    echo 'catch_workers_output = yes'; \
  } > /etc/php7/php-fpm.d/docker.conf \
  && { \
    echo '[global]'; \
    echo 'daemonize = no'; \
    echo; \
    echo '[www]'; \
    echo 'listen = [::]:9000'; \
    echo 'user = www-data'; \
    echo 'group = www-data'; \
  } > /etc/php7/php-fpm.d/zz-docker.conf \
  && { \
    # see https://secure.php.net/manual/en/opcache.installation.php
    echo 'opcache.memory_consumption=128'; \
    echo 'opcache.interned_strings_buffer=8'; \
    echo 'opcache.max_accelerated_files=4000'; \
    echo 'opcache.revalidate_freq=2'; \
    echo 'opcache.fast_shutdown=1'; \
    echo 'opcache.enable_cli=1'; \
  } > /etc/php7/conf.d/01_opcache-recommended.ini \
  && { \
    echo 'sendmail_path = "/usr/sbin/ssmtp -t"'; \
  } > /etc/php7/conf.d/01_sendmail.ini \
  && { \
    # Add .conf file to set memory limits:
    echo 'post_max_size=12M'; \
    echo 'upload_max_filesize=10M'; \
  } > /etc/php7/conf.d/01_memory.ini

# configure SSMTP and set it up for use with PHP
COPY ssmtp/* /etc/ssmtp/

VOLUME /var/www/html

COPY docker-entrypoint.sh /usr/local/bin/

ENTRYPOINT ["docker-entrypoint.sh"]
EXPOSE 9000
CMD ["php-fpm7"]
