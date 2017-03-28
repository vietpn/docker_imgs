FROM local/c7-systemd

# Install php56
RUN cd ~ && \
	rpm -ivh https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm && \
	rpm -ivh https://mirror.webtatic.com/yum/el7/webtatic-release.rpm

RUN yum -y install php56w php56w-opcache && \
	yum -y install php56w-fpm php56w-opcache php56w-mbstring php56w-mysql php56w-cli php56w-xml php56w-mcrypt && \
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

VOLUME [ "/var/log/nginx", "/var/log/php-fpm" ]

CMD ["/usr/sbin/init"]
