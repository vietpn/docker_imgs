# Build image systemd-nginx-php56
$ cd ~/c7-systemd-nginx-php56 <br />
$ docker build run -it local/c7-systemd-nginx-php56  . <br />


# Sample run docker and exec
$ docker run --name c7-systemd-nginx-php56 -p 8088:80 -v /sys/fs/cgroup:/sys/fs/cgroup -v /etc/nginx/conf_docker.d:/etc/nginx/conf.d -v /app/www:/app/www -v /app/log:/app/log -d local/c7-systemd-nginx-php56	

$ docker exec -d c7-systemd-nginx-php56 chown jenkins:www -R /app/www/