FROM php:5.6-fpm-alpine3.4

WORKDIR /var/www

MAINTAINER levonliu

#替换源
RUN sed -i 's/dl-cdn.alpinelinux.org/mirrors.aliyun.com/g' /etc/apk/repositories

#更新
RUN apk update

#安装对curl 和 png的支持
RUN apk add curl-dev libpng libpng-dev m4 autoconf gcc g++ make

#安装PHP扩展
RUN docker-php-ext-install pdo_mysql bcmath curl gd mbstring opcache &&\
    pecl install redis-3.1.3 \
    &&  rm -rf /tmp/pear \
    && docker-php-ext-enable redis

#yar
RUN curl -fsSL 'http://pecl.php.net/get/yar-1.2.4.tgz' -o yar.tgz \
    && mkdir -p yar \
    && tar -xf yar.tgz -C yar --strip-components=1 \
    && rm yar.tgz \
    && ( \
        cd yar \
        && phpize \
        && ./configure --enable-yar \
        && make -j$(nproc) \
        && make install \
    ) \
    && rm -r yar \
    && docker-php-ext-enable yar

#msgpack
RUN curl -fsSL 'http://pecl.php.net/get/msgpack-0.5.5.tgz' -o msgpack.tgz \
    && mkdir -p msgpack \
    && tar -xf msgpack.tgz -C msgpack --strip-components=1 \
    && rm msgpack.tgz \
    && ( \
        cd msgpack \
        && phpize \
        && ./configure --enable-msgpack \
        && make -j$(nproc) \
        && make install \
    ) \
    && rm -r msgpack \
    && docker-php-ext-enable msgpack

COPY ./run.sh /run.sh
RUN chmod +x /run.sh
CMD [ "/run.sh" ]