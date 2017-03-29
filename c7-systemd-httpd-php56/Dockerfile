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


# Install php56
RUN cd ~ && \
        rpm -ivh https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm && \
        rpm -ivh https://mirror.webtatic.com/yum/el7/webtatic-release.rpm

RUN yum -y install php56w php56w-opcache && \
        yum -y install php56w-fpm php56w-opcache php56w-mbstring php56w-mysql php56w-cli php56w-xml && \
        systemctl enable php-fpm

# Install httpd
RUN yum -y install httpd; yum clean all

# Install composer
RUN cd ~ && \
        yum -y install curl && \
        curl -sS https://getcomposer.org/installer | php && \
        mv composer.phar /usr/local/bin/composer && \
        ln -s /usr/local/bin/composer /usr/bin/composer

# Install PDO_OCI and OCI8
ADD oracle-instantclient12.1-basic-12.1.0.2.0-1.x86_64.rpm /root
ADD oracle-instantclient12.1-devel-12.1.0.2.0-1.x86_64.rpm /root
ADD PDO_OCI-1.0.tar /root
ADD oci8-1.4.9.tgz /root

RUN cd ~ && \
	yum -y groupinstall "Development Tools" && \
	yum -y install php56w-pear php56w-devel zlib zlib-devel bc libaio glibc && \
	yum -y install vim && \
	rpm -ivh oracle-instantclient12.1-basic-12.1.0.2.0-1.x86_64.rpm && \
	rpm -ivh oracle-instantclient12.1-devel-12.1.0.2.0-1.x86_64.rpm && \
	ln -s /usr/include/oracle/12.1/client64 /usr/include/oracle/12.1/client && \
	ln -s /usr/lib/oracle/12.1/client64 /usr/lib/oracle/12.1/client && \
	echo 'export LD_LIBRARY_PATH=/usr/lib/oracle/12.1/client64/lib' > /etc/profile.d/oracle.sh && \
	cd PDO_OCI-1.0  && \
	phpize && \
	./configure --with-pdo-oci=instantclient,/usr,12.1 && \
	make && \
	make install && \
	echo 'extension=pdo_oci.so' > /etc/php.d/pdo_oci.ini

RUN cd ~ && \
	cd oci8-1.4.9 && \
	phpize && \
	./configure --with-oci8=shared,instantclient,/usr/lib/oracle/12.1/client64/lib && \
	make && \
	make install && \
	echo 'extension=oci8.so' > /etc/php.d/oci8.ini

# Install restart httpd
RUN systemctl enable httpd.service
EXPOSE 80

# Setup projects
RUN useradd jenkins && \
    groupadd www && \
    usermod -a -G www jenkins && \
    usermod -a -G www nginx && \
    mkdir -p /app/www && \
    mkdir -p /app/log/nginx && \
    mkdir -p /app/log/httpd && \
    chown jenkins:www -R /app/www/

VOLUME [ "/sys/fs/cgroup", "/var/log/httpd", "/var/log/php-fpm", "/app/log", "app/www"]
CMD ["/usr/sbin/init"]
