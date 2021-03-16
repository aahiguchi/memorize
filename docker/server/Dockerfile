FROM php:7.2-apache

RUN curl -s https://raw.githubusercontent.com/composer/getcomposer.org/b3c61710072162971a18241e2dd2dd1e1f0e8a3c/web/installer | php -- --install-dir=/usr/local/bin/ --filename=composer

# slimの空プロジェクトをcomposer経由で作成する際に必要
RUN apt-get update && apt-get install -y \
        zip \
        unzip 


WORKDIR /var/www/html

COPY src src

# confファイルのdocument rootを書き換える
ENV APACHE_DOCUMENT_ROOT /var/www/html/src/public
RUN sed -ri -e 's!/var/www/html!${APACHE_DOCUMENT_ROOT}!g' /etc/apache2/sites-available/*.conf
RUN sed -ri -e 's!/var/www/!${APACHE_DOCUMENT_ROOT}!g' /etc/apache2/apache2.conf /etc/apache2/conf-available/*.conf
# これがないとnot reliableみたいなwarningが出る
RUN echo "ServerName localhost" >> /etc/apache2/apache2.conf
# rewriteモジュールを有効化
RUN a2enmod rewrite
# htaccessでのrewriteを許可する
RUN sed -i -e '\!<Directory ${APACHE_DOCUMENT_ROOT}>!,\!</Directory>! s!AllowOverride None!AllowOverride All!' /etc/apache2/apache2.conf