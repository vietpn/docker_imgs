# Build image c7-systemd-httpd-php56 

## Download oracle-instantclient12.1-basic-12.1.0.2.0-1.x86_64.rpm 
## Download oracle-instantclient12.1-devel-12.1.0.2.0-1.x86_64.rpm
## http://www.oracle.com/technetwork/topics/linuxx86-64soft-092277.html
## Copy driver to folder ~/c7-systemd-httpd-php56

$ cd ~/c7-systemd-httpd-php56 <br />
$ docker build run -it local/c7-systemd-httpd-php56  . <br />