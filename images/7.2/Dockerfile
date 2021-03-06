FROM php:7.2.27

# disable interactive functions
ENV DEBIAN_FRONTEND noninteractive

RUN apt-get update && \
    apt-get -y install --no-install-recommends libmcrypt-dev libreadline-dev zlib1g-dev libxml2-dev apt-utils curl git zip unzip && \
    rm -rf /var/lib/apt/lists/*

RUN docker-php-ext-install \
    mbstring \
    zip \
    pdo_mysql \
    mysqli \
    xml

RUN pecl install mcrypt-1.0.1 && \
    docker-php-ext-enable mcrypt

# Install PHP Code Sniffer
RUN curl -L https://squizlabs.github.io/PHP_CodeSniffer/phpcs.phar -o phpcs && \
    curl -L https://squizlabs.github.io/PHP_CodeSniffer/phpcbf.phar -o phpcbf && \
    chmod a+x phpcs && \
    chmod a+x phpcbf && \
    mv phpcs /usr/local/bin/phpcs && \
    mv phpcbf /usr/local/bin/phpcbf

# Install PHP Copy Paste Detector
RUN curl -L https://phar.phpunit.de/phpcpd.phar -o phpcpd.phar && \
    chmod +x phpcpd.phar && \
    mv phpcpd.phar /usr/local/bin/phpcpd

# Install PHP Dead Code Detector
RUN curl -L https://phar.phpunit.de/phpdcd.phar -o phpdcd.phar && \
    chmod +x phpdcd.phar && \
    mv phpdcd.phar /usr/local/bin/phpdcd

# Install PHP Security Checker
RUN curl -L http://get.sensiolabs.org/security-checker.phar -o security-checker.phar && \
    chmod +x security-checker.phar && \
    mv security-checker.phar /usr/local/bin/security-checker

# Install Composer
RUN curl -sS https://getcomposer.org/installer -o composer-setup.php && \
    php composer-setup.php --install-dir=/usr/local/bin --filename=composer && \
    composer config -g repo.packagist composer https://packagist.phpcomposer.com

# Install PhpMetrics
RUN curl -OL https://github.com/phpmetrics/PhpMetrics/releases/download/v2.3.2/phpmetrics.phar && \
    chmod +x phpmetrics.phar && mv phpmetrics.phar /usr/local/bin/phpmetrics

# Install phpcs-security-audit v2 sniffes
RUN mkdir -p /opt/phpcs/snifss/ && \
    cd /opt/phpcs/snifss/ && \
    git clone -b master https://github.com/FloeDesignTechnologies/phpcs-security-audit.git phpcs-security-audit && \
    phpcs --config-set installed_paths /opt/phpcs/snifss/phpcs-security-audit/

# Create tools folder
RUN mkdir -p /tools

# Set global bin-dir for composer packages
ENV COMPOSER_BIN_DIR="/usr/local/bin"

# Install Psecio Parse
RUN mkdir -p /tools/psecio/parse && \
    cd /tools/psecio/parse && \
    composer require --dev psecio/parse && \
    cd -

# Install PHP-Var-Dump-Check
RUN mkdir -p /tools/jakub-onderka/php-var-dump-check && \
    cd /tools/jakub-onderka/php-var-dump-check && \
    composer require --dev jakub-onderka/php-var-dump-check && \
    cd -

# Install PHP-Parallel-Lint
RUN mkdir -p /tools/jakub-onderka/php-parallel-lint && \
    cd /tools/jakub-onderka/php-parallel-lint && \
    composer require --dev jakub-onderka/php-parallel-lint && \
    cd -

# Install Magic Number Detecter
RUN mkdir -p /tools/povils/phpmnd && \
    cd /tools/povils/phpmnd && \
    composer require povils/phpmnd && \
    cd -

# Install composer.json fixer
RUN mkdir -p /tools/kubawerlos/composer-json-fixer && \
    cd /tools/kubawerlos/composer-json-fixer && \
    composer require kubawerlos/composer-json-fixer && \
    cd -

# Install PHP Assumptions
RUN mkdir -p /tools/rskuipers/php-assumptions && \
    cd /tools/rskuipers/php-assumptions && \
    composer require --dev rskuipers/php-assumptions && \
    cd -

# Install churn-php
RUN mkdir -p /tools/bmitch/churn-php && \
    cd /tools/bmitch/churn-php && \
    composer require --dev bmitch/churn-php && \
    cd -

# Install Fink
RUN mkdir -p /tools/dantleech/fink && \
    cd /tools/dantleech/fink && \
    composer require --dev dantleech/fink && \
    cd -

# Add update script
ADD https://raw.githubusercontent.com/umutphp/php-docker-images-for-ci/master/scripts/update.sh /update.sh
RUN chmod +x /update.sh

# Add gitignore check script
ADD https://raw.githubusercontent.com/umutphp/php-docker-images-for-ci/master/scripts/gitignore_checker.sh /gitignore_checker.sh
RUN chmod +x /gitignore_checker.sh && ln -s /gitignore_checker.sh /usr/local/bin/gitignore_checker

# Add merge conflict checker script
ADD https://raw.githubusercontent.com/umutphp/php-docker-images-for-ci/master/scripts/merge_conflict_checker.sh /merge_conflict_checker.sh
RUN chmod +x /merge_conflict_checker.sh && ln -s /merge_conflict_checker.sh /usr/local/bin/merge_conflict_checker

# Install PHPMD
RUN mkdir -p /tools/phpmd/phpmd && \
    cd /tools/phpmd/phpmd && \
    composer require --dev phpmd/phpmd && \
    cd -

# Install PHP_Testability
RUN mkdir -p /tools/edsonmedina/php_testability && \
    cd /tools/edsonmedina/php_testability && \
    composer require --dev edsonmedina/php_testability:dev-master && \
    cd -

# Install composer-normalize
RUN composer global require localheinz/composer-normalize

# Install infection/infection for mutation testing
RUN pecl install xdebug-2.6.0alpha1 && docker-php-ext-enable xdebug
RUN composer require infection/infection

# Install Deptrac
RUN mkdir -p /tools/deptrac && \
    curl -LS https://github.com/sensiolabs-de/deptrac/releases/download/0.6.0/deptrac.phar -o deptrac.phar && \
    chmod +x deptrac.phar && mv deptrac.phar /tools/deptrac/deptrac

# Install PHP Insights
RUN mkdir -p /tools/nunomaduro/phpinsights && \
    cd /tools/nunomaduro/phpinsights && \
    composer global require nunomaduro/phpinsights && \
    cd -

# Unset COMPOSER_BIN_DIR
ENV COMPOSER_BIN_DIR=""

# Unset the entry point
ENTRYPOINT []
