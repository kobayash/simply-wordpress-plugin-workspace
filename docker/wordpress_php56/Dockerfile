FROM php:5.6-apache

# install the PHP extensions we need
RUN set -ex; \
	\
	apt-get update; \
	apt-get install -y \
		libjpeg-dev \
		libpng12-dev \
		less \
		subversion \
		git \
		zip \
		unzip \
	; \
	echo "mysql-server-5.5 mysql-server/root_password password root" | debconf-set-selections \
		&& echo "mysql-server-5.5 mysql-server/root_password_again password root" | debconf-set-selections \
		&& apt-get install -y mysql-server \
	; \
	rm -rf /var/lib/apt/lists/*; \
	\
	docker-php-ext-configure gd --with-png-dir=/usr --with-jpeg-dir=/usr; \
	docker-php-ext-install gd mysqli opcache

# set recommended PHP.ini settings
# see https://secure.php.net/manual/en/opcache.installation.php
RUN { \
		echo 'opcache.memory_consumption=128'; \
		echo 'opcache.interned_strings_buffer=8'; \
		echo 'opcache.max_accelerated_files=4000'; \
		echo 'opcache.revalidate_freq=2'; \
		echo 'opcache.fast_shutdown=1'; \
		echo 'opcache.enable_cli=1'; \
		echo 'SMTP=mail'; \
		echo 'smtp_port=1025'; \
		
	} > /usr/local/etc/php/conf.d/opcache-recommended.ini

RUN a2enmod rewrite expires

RUN curl -sSL https://getcomposer.org/installer | php \
   && mv composer.phar /usr/local/bin/composer

RUN curl -O https://raw.githubusercontent.com/wp-cli/builds/gh-pages/phar/wp-cli.phar \
	&& chmod +x wp-cli.phar \
	&& mv wp-cli.phar /usr/local/bin/wp

RUN	curl -O https://phar.phpunit.de/phpunit-4.8.9.phar \
    && chmod +x phpunit-4.8.9.phar \
    && mv phpunit-4.8.9.phar /usr/local/bin/phpunit

RUN useradd -m -d /home/docker -s /bin/bash docker \
 && echo "docker:docker" | chpasswd \
 && mkdir /home/docker/.ssh \
 && chmod 700 /home/docker/.ssh \
 && chown -R docker:docker /home/docker/.ssh \ 
 && chmod go+w /var/www/html

RUN echo "docker ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers

CMD ["apache2-foreground"]
