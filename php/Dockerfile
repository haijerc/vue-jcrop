FROM php:7.2-fpm
RUN apt-get update && apt-get install -y \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libpng-dev \
        wget \
        git \
        zip \
    && docker-php-ext-install -j$(nproc) iconv \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install -j$(nproc) gd 

RUN apt-get install -y libcurl4-openssl-dev libxml2-dev libxrender-dev libfontconfig \
  && git clone https://github.com/php/pecl-search_engine-solr.git --depth=1 \
  && cd pecl-search_engine-solr \
  && /usr/local/bin/phpize && ./configure && make && make install \
  && cd .. && rm -rf pecl-search_engine-solr \
  && rm -r /var/lib/apt/lists/* \
  && docker-php-ext-enable solr

RUN docker-php-ext-install pdo_mysql pdo mysqli

RUN docker-php-ext-install opcache

COPY ./opcache.ini /usr/local/etc/php/conf.d/opcache.ini

RUN \
  curl 'http://pecl.php.net/get/redis-3.1.5.tgz' -o /tmp/redis-3.1.5.tgz  \
  && cd /tmp \
  && pecl install redis-3.1.5.tgz \
  && rm -rf redis-3.1.5.tgz \
  && docker-php-ext-enable redis \

  && pecl install xdebug \
  && docker-php-ext-enable xdebug

COPY ./xdebug.ini /usr/local/etc/php/conf.d/xdebug2.ini

COPY dart-sass-1.43.2-linux-x64.tar.gz /usr/local/src

RUN tar -xvf /usr/local/src/dart-sass-1.43.2-linux-x64.tar.gz -C /usr/local/src/ \
  && cp /usr/local/src/dart-sass/sass /usr/local/bin/dsass