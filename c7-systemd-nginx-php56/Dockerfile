FROM local/c7-systemd

# Install php56
RUN cd ~ && \
	rpm -ivh https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm && \
	rpm -ivh https://mirror.webtatic.com/yum/el7/webtatic-release.rpm

RUN yum -y install php56w php56w-opcache && \
	yum -y install php56w-fpm php56w-opcache php56w-mbstring php56w-mysql php56w-cli php56w-xml && \
	systemctl enable php-fpm

# Install composer
RUN cd ~ && \
	yum -y install curl && \
	curl -sS https://getcomposer.org/installer | php && \
	mv composer.phar /usr/local/bin/composer && \
	ln -s /usr/local/bin/composer /usr/bin/composer


# Install nginx
RUN yum -y install epel-release && \
	yum -y install nginx && \ 
	yum clean all && \
        systemctl enable nginx
EXPOSE 80 443

# Setup projects
RUN useradd jenkins && \
    groupadd www && \
    usermod -a -G www jenkins && \
    usermod -a -G www nginx && \
    mkdir -p /app/www && \
    mkdir -p /app/log/nginx && \
    mkdir -p /app/log/httpd && \
    chown jenkins:www -R /app/www/

VOLUME [ "/var/log/nginx", "/var/log/php-fpm", "/app/log", "app/www"]

CMD ["/usr/sbin/init"]
