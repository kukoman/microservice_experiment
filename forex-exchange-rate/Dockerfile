FROM muteor/linkerd-nginx-php

# Copy src files to non public dir
COPY src /var/www/src

# Copy public entrypoint
COPY public /var/www/html

# Install deps
RUN export DEBIAN_FRONTEND=noninteractive && \
    apt-get update -q && \
    apt-get install -yq php7.0-zip php7.0-simplexml && \
    rm -rf /var/lib/apt/lists/*
COPY composer.json /var/www
COPY composer.lock /var/www
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer && \
    cd /var/www && \
    composer install

# Sidecar linkerd
COPY conf/linkerd.yaml /etc/linkerd/linkerd.yaml

# ADD ECB rates data
ADD https://www.ecb.europa.eu/stats/eurofxref/eurofxref-hist-90d.xml /var/www/data/rates.xml
RUN chown -R www-data:www-data /var/www/data

EXPOSE 4141
