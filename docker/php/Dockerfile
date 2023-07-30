FROM php:7.4.0-fpm-alpine

WORKDIR /var/www
RUN rm -rf /var/www/html
COPY . /var/www
RUN ln -s public html

RUN apk add --no-cache shadow openssl vim bash mysql-client nodejs npm git $PHPIZE_DEPS
RUN docker-php-ext-install pdo pdo_mysql

RUN pecl install xdebug-3.1.5

RUN touch /home/www-data/.bashrc | echo "PS1='\w\$ '" >> /home/www-data/.bashrc

# caminho lido pelo php fpm (web)
COPY xdebug.ini  /etc/php7/conf.d/
# caminho lido pelo php cli
COPY xdebug.ini  $PHP_INI_DIR/conf.d/

RUN echo "zend_extension=xdebug.so" >> /etc/php7/php.ini

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer
RUN usermod -u 1000 www-data

EXPOSE 9002
ENTRYPOINT ["php-fpm"]