FROM phpdockerio/php56-cli

# Install dotdeb repo, PHP, composer and selected extensions
RUN apt-get update \
    && apt-get -y --no-install-recommends install php5-fpm \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* /usr/share/doc/*

RUN apt-get update && apt-get install php5-xdebug -y

RUN mkdir /run/php
ADD conf/www.conf /etc/php5/fpm/pool.d/www.conf
ADD conf/php-fpm.conf /etc/php5/fpm/php-fpm.conf
RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
RUN php composer-setup.php
RUN php -r "unlink('composer-setup.php');"
RUN mv composer.phar /usr/local/bin/composer
RUN chmod +x /usr/local/bin/composer
RUN ln -snf /usr/share/zoneinfo/Europe/Berlin /etc/localtime
RUN echo "xdebug.idekey = PHPSTORM" >> /etc/php5/fpm/conf.d/20-xdebug.ini
RUN echo "xdebug.default_enable=1" >> /etc/php5/fpm/conf.d/20-xdebug.ini
RUN echo "xdebug.remote_enable=1" >> /etc/php5/fpm/conf.d/20-xdebug.ini
RUN echo "xdebug.remote_autostart = 1" >> /etc/php5/fpm/conf.d/20-xdebug.ini
RUN echo "xdebug.remote_port = 9001" >> /etc/php5/fpm/conf.d/20-xdebug.ini
RUN echo "xdebug.remote_handler = dbgp" >> /etc/php5/fpm/conf.d/20-xdebug.ini
RUN echo "xdebug.remote_host = $(getent hosts docker.for.mac.localhost | awk '{ print $1 }')" >> /etc/php5/fpm/conf.d/20-xdebug.ini


# The following runs FPM and removes all its extraneous log output on top of what your app outputs to stdout
CMD /usr/sbin/php5-fpm -F -O 2>&1 | sed -u 's,.*: \"\(.*\)$,\1,'| sed -u 's,"$,,' 1>&1

# Open up fcgi port
EXPOSE 9000


