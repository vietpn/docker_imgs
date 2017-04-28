FROM centos:7
MAINTAINER "vietpn" <vietpnk53@gmail.com>
ENV container docker
RUN (cd /lib/systemd/system/sysinit.target.wants/; for i in *; do [ $i == \
systemd-tmpfiles-setup.service ] || rm -f $i; done); \
rm -f /lib/systemd/system/multi-user.target.wants/*;\
rm -f /etc/systemd/system/*.wants/*;\
rm -f /lib/systemd/system/local-fs.target.wants/*; \
rm -f /lib/systemd/system/sockets.target.wants/*udev*; \
rm -f /lib/systemd/system/sockets.target.wants/*initctl*; \
rm -f /lib/systemd/system/basic.target.wants/*;\
rm -f /lib/systemd/system/anaconda.target.wants/*;

# install common packages
RUN yum -y install vim && \
    yum -y groupinstall "Development Tools" && \

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

# Install memcache on centos
RUN yum -y install memcached && \
    systemctl enable memcached

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

VOLUME [ "/sys/fs/cgroup", "/var/log/nginx", "/var/log/php-fpm", "/app/log", "app/www"]

CMD ["/usr/sbin/init"]
