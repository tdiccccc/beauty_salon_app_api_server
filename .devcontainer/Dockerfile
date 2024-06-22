# イメージを取得
FROM php:8.1-fpm

# VITEのためにポートの穴あけ
EXPOSE 5173

# 必要なパッケージやPHPの拡張モジュールをインストールするコマンド
RUN apt-get update \
    && apt-get install -y \
        libpng-dev \
        libjpeg-dev \
        libfreetype6-dev \
        libzip-dev \
        zlib1g-dev \
        libmcrypt-dev \
        libxi6 \
        curl \
        git \
        unzip \
        vim \
        gnupg \
        libreoffice \
        fonts-ipafont-gothic \
    && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# PHPの拡張モジュールをインストール
RUN docker-php-ext-configure gd --with-freetype --with-jpeg \
    && docker-php-ext-install -j$(nproc) gd pdo_mysql bcmath zip

# PHP設定を追加
RUN echo "file_uploads = On\n" \
    "memory_limit = -1\n" \
    "upload_max_filesize = 500M\n" \
    "post_max_size = 500M\n" \
    "max_execution_time = 600\n" \
    > /usr/local/etc/php/conf.d/docker-php-ext-uploads.ini

# Composerのインストール
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Composerの設定
ENV COMPOSER_ALLOW_SUPERUSER 1
ENV COMPOSER_HOME /composer
ENV PATH $PATH:/composer/vendor/bin

# 作業ディレクトリの設定
WORKDIR /var/www/api-server

# PHP-FPMのデフォルトポートの公開
EXPOSE 9000

# コンテナ起動時のコマンド
CMD ["php-fpm"]
