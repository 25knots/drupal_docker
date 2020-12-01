FROM drupal:8.8.6
MAINTAINER mail@jaapjan.nl

ARG APP_ENVIRONMENT=prod

# Install packages.
RUN apt-get update && apt-get install -y \
  libzip-dev \
  zlib1g-dev \
  mariadb-client \
  git \
  msmtp \
  nano \
  vim && \
  apt-get clean

ADD mailcatcher-ssmtp.conf /etc/ssmtp/ssmtp.conf

RUN echo 'sendmail_path = "/usr/sbin/ssmtp -t"' > /usr/local/etc/php/conf.d/mail.ini

ADD php.ini /usr/local/etc/php/php.ini

# Install extensions
RUN docker-php-ext-install zip bcmath exif

# Install Composer.
RUN curl -sS https://getcomposer.org/installer | php
RUN mv composer.phar /usr/local/bin/composer

WORKDIR /var/www/
ENV COMPOSER_ALLOW_SUPERUSER=1

WORKDIR /var/www/html/
RUN chown -R www-data:www-data *

# Unfortunately, adding the composer vendor dir to the PATH doesn't seem to work. So:
RUN ln -s /var/www/vendor/bin/drush /usr/local/bin/drush

RUN php -r 'opcache_reset();'

# Fix shell.
RUN echo "export TERM=xterm" >> ~/.bashrc

# Xdebug.
RUN if [ "$APP_ENVIRONMENT" = "dev" ] ; then pecl install xdebug ; fi
RUN if [ "$APP_ENVIRONMENT" = "dev" ] ; then docker-php-ext-enable xdebug; fi
RUN if [ "$APP_ENVIRONMENT" = "dev" ] ; then echo 'xdebug.remote_autostart=true' >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini; fi
RUN if [ "$APP_ENVIRONMENT" = "dev" ] ; then echo 'xdebug.remote_port=9001' >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini; fi
RUN if [ "$APP_ENVIRONMENT" = "dev" ] ; then echo 'xdebug.mode=debug' >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini; fi
RUN if [ "$APP_ENVIRONMENT" = "dev" ] ; then echo 'xdebug.client_port=9001' >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini; fi
RUN if [ "$APP_ENVIRONMENT" = "dev" ] ; then echo 'xdebug.client_host=host.docker.internal' >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini; fi
RUN if [ "$APP_ENVIRONMENT" = "dev" ] ; then echo 'xdebug.remote_host=host.docker.internal' >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini; fi
RUN if [ "$APP_ENVIRONMENT" = "dev" ] ; then echo 'xdebug.remote_enable=1' >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini; fi
RUN if [ "$APP_ENVIRONMENT" = "dev" ] ; then echo 'xdebug.idekey=PHPSTORM' >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini; fi

## Settings to test later:
#RUN if [ "$APP_ENVIRONMENT" = "dev" ] ; then echo 'xdebug.remote_mode=req' >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini; fi
#RUN if [ "$APP_ENVIRONMENT" = "dev" ] ; then echo 'xdebug.remote_handler=dbgp' >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini; fi
#RUN if [ "$APP_ENVIRONMENT" = "dev" ] ; then echo 'xdebug.remote_connect_back=1 ' >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini; fi
#RUN if [ "$APP_ENVIRONMENT" = "dev" ] ; then echo 'xdebug.extended_info=1' >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini; fi

## To make it (NOT) work with drush/php scripts.
# RUN if [ "$APP_ENVIRONMENT" = "dev" ] ; then export XDEBUG_CONFIG="idekey=PHPSTORM remote_enable=1 remote_mode=req remote_port=9000 remote_host=host.docker.internal"; fi
# RUN if [ "$APP_ENVIRONMENT" = "dev" ] ; then export PHP_IDE_CONFIG="serverName=waychanger"; fi
