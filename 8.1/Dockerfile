FROM php:8.1

LABEL maintainer="Kurt Friars"
LABEL site="https://github.com/kfriars/laravel-cypress-docker"

ENV CHROME_VERSION 94.0.4606.71
ENV FIREFOX_VERSION 93.0
ENV DEBIAN_FRONTEND=noninteractive
ENV TERM xterm
ENV npm_config_loglevel warn
ENV npm_config_unsafe_perm true
ENV DBUS_SESSION_BUS_ADDRESS=/dev/null
ENV CI=1
ENV CYPRESS_CACHE_FOLDER=/root/.cache/Cypress

RUN apt update && apt install -y \
    $PHPIZE_DEPS \
    sqlite3 \
    bash \
    curl \
    libfreetype-dev \
    g++ \
    gcc \
    git \
    icu-devtools \
    imagemagick \
    libmagickwand-dev \
    libcurl4 \
    libcurl4-openssl-dev \
    libicu-dev \
    libc-dev \
    libturbojpeg-dev \
    libpng-dev \
    libpq-dev \
    libsqlite3-dev \
    libtool \
    libxml2-dev \
    libzip-dev \
    make \
    mariadb-client \
    nodejs \
    npm \
    libonig-dev \
    yarn \
    openssh-client \
    postgresql-client \
    python \
    rsync \
    zlib1g-dev \
    # Cypress dependencies
    libgtk2.0-0 \
    libgtk-3-0 \
    libgbm-dev \
    libnotify-dev \
    libgconf-2-4 \
    libnss3 \
    libxss1 \
    libasound2 \
    libxtst6 \
    xauth \
    xvfb \
    # Extra dependencies
    fonts-liberation \
    xdg-utils \
    mplayer \
    apt-utils \
    wget

# Install PECL and PEAR extensions
RUN pecl install \
    redis \
    imagick \
    xdebug

# Enable PECL and PEAR extensions
RUN docker-php-ext-enable \
    redis \
    imagick \
    xdebug

# Configure php extensions
RUN docker-php-ext-configure gd --with-freetype --with-jpeg

# Install php extensions
RUN docker-php-ext-install \
    bcmath \
    calendar \
    curl \
    exif \
    gd \
    iconv \
    intl \
    mbstring \
    pdo \
    pdo_mysql \
    pdo_pgsql \
    pdo_sqlite \
    pcntl \
    soap \
    xml \
    zip

# Install composer
ENV COMPOSER_HOME /composer
ENV PATH ./vendor/bin:/composer/vendor/bin:$PATH
ENV COMPOSER_ALLOW_SUPERUSER 1
RUN curl -s https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin/ --filename=composer

# Install PHP_CodeSniffer
RUN composer global require "squizlabs/php_codesniffer=*"

# Setup working directory
WORKDIR /var/www

# Chrome
RUN wget -O /usr/src/google-chrome-stable_current_amd64.deb "http://dl.google.com/linux/chrome/deb/pool/main/g/google-chrome-stable/google-chrome-stable_${CHROME_VERSION}-1_amd64.deb" && \
    dpkg -i /usr/src/google-chrome-stable_current_amd64.deb ; \
    apt-get install -f -y && \
    rm -f /usr/src/google-chrome-stable_current_amd64.deb

# Firefox
RUN wget --no-verbose -O /tmp/firefox.tar.bz2 "https://download-installer.cdn.mozilla.net/pub/firefox/releases/$FIREFOX_VERSION/linux-x86_64/en-US/firefox-$FIREFOX_VERSION.tar.bz2" \
    && tar -C /opt -xjf /tmp/firefox.tar.bz2 \
    && rm /tmp/firefox.tar.bz2 \
    && ln -fs /opt/firefox/firefox /usr/bin/firefox
