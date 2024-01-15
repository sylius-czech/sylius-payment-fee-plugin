ARG PHP_VERSION=8.0

FROM webdevops/php-nginx:${PHP_VERSION}

ARG DEBIAN_FRONTEND=noninteractive
ARG NODE_VERSION=18

ENV LC_ALL=C.UTF-8
ENV XDEBUG_MODE=off
ENV SYLIUS_VERSION=${SYLIUS_VERSION}

RUN apt-get update

RUN apt-get install -y nodejs yarn mariadb-client netcat-traditional curl

# to avoid installing different package "Note, selecting 'cmdtest' instead of 'yarn'"
RUN curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add -

RUN echo "deb https://dl.yarnpkg.com/debian/ stable main" | tee /etc/apt/sources.list.d/yarn.list

RUN curl -fsSL https://deb.nodesource.com/setup_${NODE_VERSION}.x | bash -

RUN apt-get clean && rm -rf /var/lib/apt/lists/*

RUN curl -sS https://get.symfony.com/cli/installer | bash -s -- --install-dir="$(pwd)" && mv "$(pwd)"/symfony /usr/local/bin/symfony

RUN symfony server:ca:install

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/bin --filename composer

RUN composer require "sylius/sylius:${SYLIUS_VERSION}.*" --no-interaction --no-update --no-scripts
