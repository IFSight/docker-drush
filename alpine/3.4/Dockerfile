FROM fulcrum/php:latest-alpine
MAINTAINER IF Fulcrum "fulcrum@ifsight.net"

RUN apk --update add curl git php5-phar php5-openssl                       && \
    cd /usr/local                                                          && \
    curl -sS https://getcomposer.org/installer | php                       && \
    /bin/mv composer.phar bin/composer                                     && \
    adduser -h /tmp/phphome -s /bin/sh -D -H -u 1971 php                   && \
    mkdir -p /usr/share/drush/commands/ /tmp/phphome drush7 drush8         && \
    chown php.php /tmp/phphome drush7 drush8                               && \
    su - php -c "cd /usr/local/drush7 && composer require drush/drush:7.*" && \
    su - php -c "cd /usr/local/drush8 && composer require drush/drush:8.*" && \
    ln -s /usr/local/drush8/vendor/drush/drush/drush /usr/local/bin/drush  && \
    ln -s /usr/local/drush7/vendor/drush/drush/drush /usr/local/bin/drush7 && \
    ln -s /usr/local/drush8/vendor/drush/drush/drush /usr/local/bin/drush8 && \
    su - php -c "/usr/local/bin/drush @none dl registry_rebuild"           && \
    deluser php                                                            && \
    adduser -h /var/www/html -s /bin/sh -D -H -u 1971 php

USER php

# Move to the directory were the php files stands
WORKDIR /var/www/html

ENTRYPOINT ["/usr/local/bin/drush"]
