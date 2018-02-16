FROM wordpress:php7.1-fpm

RUN set -ex; \
	apt-get update; \
	apt-get install -y \
	imagemagick \
	less \
	libfreetype6-dev \
	libjpeg62-turbo-dev \
	libmagickwand-dev \
	libmemcached-dev \
	libpcre3 \
	libpcre3-dev \
	libpng12-dev \
	libssl-dev \
	libz-dev \
	ssmtp \
	unzip; \
	rm -rf /var/lib/apt/lists/*;

RUN pecl install imagick
RUN docker-php-ext-enable imagick

# Install Memcached
RUN apt-get update -y \ 
    && apt-get install git -y \
    && cd ~ \
    && git clone https://github.com/php-memcached-dev/php-memcached.git \
    && cd php-memcached \
    && git checkout php7 \
    && phpize \
    && ./configure --disable-memcached-sasl \
    && make \
    && make install \
    && echo "extension=memcached.so" > /usr/local/etc/php/conf.d/ext-memcached.ini 

# Donwload and install composer
RUN curl -sSL "https://getcomposer.org/installer" | php \
	&& mv composer.phar /usr/local/bin/composer

# Install wp-cli
RUN curl -O "https://raw.githubusercontent.com/wp-cli/builds/gh-pages/phar/wp-cli.phar" \
	&& chmod +x wp-cli.phar \
	&& mv wp-cli.phar /usr/local/bin/wp

# Setup a config file
RUN mkdir -p /etc/wp-cli
RUN { \
	echo 'path: /var/www/html'; \
	} > /etc/wp-cli/config.yml

RUN echo "mailhub=mail:1025\nUseTLS=NO\nFromLineOverride=YES" > /etc/ssmtp/ssmtp.conf
