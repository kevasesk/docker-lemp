FROM php:7.3-fpm
RUN apt-get update && apt-get install -y git zip vim curl libxml2-dev libmcrypt-dev libpng-dev libzip-dev  libfreetype6-dev libjpeg62-turbo-dev gcc make autoconf libc-dev pkg-config cron sendmail
RUN curl --silent --show-error https://getcomposer.org/installer | php && mv composer.phar /usr/local/bin/composer
RUN pecl install mcrypt-1.0.2

# Install extensions using the helper script provided by the base image
RUN docker-php-ext-install \
	zip \
	pdo_mysql \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
	&& docker-php-ext-install -j$(nproc) gd \
	&& docker-php-ext-install mysqli \
	&& docker-php-ext-install soap

#Install node
RUN curl https://nodejs.org/dist/v16.14.2/node-v16.14.2-linux-x64.tar.xz --output node.tar.xz \
    && mkdir /usr/local/lib/node \
    && tar -xJvf node.tar.xz \
    && mv node-v16.14.2-linux-x64 /usr/local/lib/node/nodejs \
    && echo "export NODEJS_HOME=/usr/local/lib/node/nodejs" >> ~/.profile \
    && echo "export PATH=\$NODEJS_HOME/bin:\$PATH" >> ~/.profile \
    && . ~/.profile

RUN mv "$PHP_INI_DIR/php.ini-production" "$PHP_INI_DIR/php.ini"

EXPOSE 9000