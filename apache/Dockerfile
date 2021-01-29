FROM php:7.4-apache-buster

RUN apt-get update && apt-get install -y vim git zip unzip libpq-dev libxml2-dev wget libmcrypt-dev mc imagemagick
RUN apt-get install -y libwebp-dev libjpeg62-turbo-dev libpng-dev libxpm-dev \
    libfreetype6-dev zlib1g-dev libzip-dev

RUN docker-php-ext-install gd
RUN docker-php-ext-install sockets
RUN apt-get install -y netcat ssh iputils-ping

RUN apt-get install -y locales locales-all
ENV LC_ALL en_US.utf-8
ENV LANG en_US.utf-8
ENV LANGUAGE en_US.utf-8

RUN apt install -y curl software-properties-common
RUN curl -sL https://deb.nodesource.com/setup_12.x | bash -
RUN apt install -y nodejs

RUN docker-php-ext-install pdo pdo_pgsql pgsql soap bcmath zip
RUN docker-php-ext-install mysqli pdo pdo_mysql
RUN apt-get install -y nmap
RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
RUN php composer-setup.php
RUN chmod +x composer.phar
RUN mv composer.phar /usr/bin/composer

RUN apt-get update && apt-get install -y \
    libldap2-dev
RUN docker-php-ext-configure ldap --with-libdir=lib/x86_64-linux-gnu/
RUN docker-php-ext-install ldap

RUN docker-php-ext-install calendar exif gettext intl pcntl shmop sysvmsg sysvsem sysvshm

RUN apt-get update && apt-get install -y libbz2-dev
RUN docker-php-ext-install bz2

RUN apt install -y libxslt-dev
RUN docker-php-ext-install xsl

# Install OPCache
RUN docker-php-ext-configure opcache --enable-opcache \
    && docker-php-ext-install opcache
##################
RUN a2enmod headers && a2enmod rewrite && a2enmod expires && a2enmod ssl

RUN apt-get install --fix-missing -y xvfb
RUN apt-get -y install cron
RUN yes | pecl install xdebug

RUN echo "zend_extension=$(find /usr/local/lib/php/extensions/ -name xdebug.so)" > /usr/local/etc/php/conf.d/xdebug.ini \
&& echo "xdebug.mode=debug" >> /usr/local/etc/php/conf.d/xdebug.ini \
&& echo "xdebug.client_port=9000" >> /usr/local/etc/php/conf.d/xdebug.ini \
&& echo "xdebug.client_host=172.17.0.1" >> /usr/local/etc/php/conf.d/xdebug.ini \
&& echo "xdebug.start_with_request=yes" >> /usr/local/etc/php/conf.d/xdebug.ini \
&& echo "xdebug.discover_client_host=0" >> /usr/local/etc/php/conf.d/xdebug.ini \
&& echo "xdebug.idekey=PHPSTORM" >> /usr/local/etc/php/conf.d/xdebug.ini \
&& echo "xdebug.show_error_trace=1" >> /usr/local/etc/php/conf.d/xdebug.ini \
&& echo "xdebug.max_nesting_level=250" >> /usr/local/etc/php/conf.d/xdebug.ini \
&& echo "xdebug.var_display_max_depth=10" >> /usr/local/etc/php/conf.d/xdebug.ini \
&& echo "xdebug.log=/var/log/xdebug.log" >> /usr/local/etc/php/conf.d/xdebug.ini

COPY ./etc /etc

RUN a2ensite example.conf \
&& a2ensite example1.conf \
&& a2ensite example2.conf \
&& a2ensite example3.conf \
&& a2ensite example4.conf \
&& a2ensite example5.conf \
&& a2ensite example6.conf \
&& a2ensite example7.conf

COPY ./usr /usr

RUN /etc/init.d/apache2 restart
