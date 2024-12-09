# Базовый образ PHP с FPM
FROM php:8.2-fpm

# Установка необходимых пакетов и расширений PHP
RUN apt-get update && apt-get install -y \
      apt-utils \
      libpq-dev \
      libpng-dev \
      libzip-dev \
      zip unzip \
      git && \
      docker-php-ext-install pdo_mysql bcmath gd zip && \
      apt-get clean && \
      rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# Копирование пользовательского файла конфигурации PHP
COPY ./_docker/app/php.ini /usr/local/etc/php/conf.d/php.ini

# Установка рабочей директории
WORKDIR /var/www

RUN echo "Before copying:" && ls -l .

# Копирование файлов проекта
COPY app            /var/www/app
COPY bootstrap      /var/www/bootstrap
COPY config         /var/www/config
COPY database       /var/www/database
COPY public         /var/www/public
COPY resources      /var/www/resources
COPY routes         /var/www/routes
COPY storage        /var/www/storage
COPY artisan        /var/www/artisan
#COPY package.json   /var/www/package.json

RUN echo "After copying:" && ls -l /var/www

RUN echo "alias a='artisan'" >> /root/.bashrc

# Копирование Composer из другого образа
COPY --from=composer:2.2 /usr/bin/composer /usr/bin/composer

# Копирование файлов Composer
COPY composer.json composer.lock ./

# Установка прав доступа перед запуском Composer
RUN chown -R www-data:www-data /var/www && \
    chmod 755 -R /var/www && \
    chmod -R 775 /var/www/storage && \
    chmod -R 775 /var/www/bootstrap/cache

# Выполнение Composer install под пользователем www-data
USER www-data

RUN composer install --optimize-autoloader --no-interaction --no-scripts




