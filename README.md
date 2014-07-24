**Description**  
This repository contains a collection of Docker configurations I've put together to meet my needs.

**Directory Structure**  
Nginx and Nginx + PHP-FPM have a simple directory structure that can be used to simply and easily deploy web applications using a volume binding on /data.

    /data
        /conf
            nginx-*.conf // included by nginx
            php-*.conf // included by php-fpm
        /http
            index.html // root web directory (index file is index.html or index.php)
        /logs
            nginx.log // nginx log file
            php-fpm.log // php-fpm log file
        /secure
            filename.ext // private files such as passwords or keys

**Usage**  
The following commands can be used to deploy some of the services offered by the Docker containers in this repository.

- **Applications**

  - **Adminer**

          docker run --name="adminer" -d --link mariadb:mariadb --link postgresql:postgresql -e VIRTUAL_HOST=adminer.example.com maxexcloo/adminer

  - **Dnsmasq**

          docker run --name="dnsmasq-data" maxexcloo/data
          docker run --name="dnsmasq" -it --privileged --volumes-from="dnsmasq-data" -p 53:53 maxexcloo/dnsmasq

  - **HAProxy**

          docker run --name="haproxy-data" maxexcloo/data
          docker run --name="haproxy-config" -it -v /var/run/docker.sock:/tmp/docker.sock maxexcloo/haproxy-config
          docker run --name="haproxy" -it --volumes-from="haproxy-config" --volumes-from="haproxy-data" -p 80:80 -p 443:443 maxexcloo/haproxy

  - **Minecraft**

          docker run --name="minecraft-data" maxexcloo/data
          docker run --name="minecraft" -it --volumes-from="minecraft-data" -e MEMORY=1024 -p 25565:25565 maxexcloo/minecraft

  - **phpMyAdmin**

          docker run --name="phpmyadmin" -d --link mariadb:mariadb -e VIRTUAL_HOST=phpmyadmin.example.com maxexcloo/phpmyadmin

  - **phpPgAdmin**

          docker run --name="phppgadmin" -d --link postgresql:postgresql -e VIRTUAL_HOST=phppgadmin.example.com maxexcloo/phppgadmin

  - **SNI Proxy**

          docker run --name="sniproxy-data" maxexcloo/data
          docker run --name="sniproxy" -it --volumes-from="sniproxy-data" -p 80:80 -p 443:443 maxexcloo/sniproxy

  - **Tiny Tiny RSS**

          docker run --name="tiny-tiny-rss-data" maxexcloo/data
          docker run --name="tiny-tiny-rss" -it --link postgresql:postgresql --volumes-from="tiny-tiny-rss-data" -e VIRTUAL_HOST=tiny-tiny-rss.example.com maxexcloo/tiny-tiny-rss

  - **ZNC**

          docker run --name="znc-data" maxexcloo/data
          docker run --name="znc" -it --volumes-from="znc-data" -e VIRTUAL_HOST=znc.example.com -e VIRTUAL_PORT=6667 -p 6697:6697 -p 6667:6667 maxexcloo/znc

  - **WebIRC**

          docker run --name="webirc" -it --link znc:znc -e VIRTUAL_HOST=webirc.example.com -e VIRTUAL_PORT=8080 -p 8080:8080 maxexcloo/webirc

- **Services**

  - **nginx**
	
          docker run --name="nginx-data" maxexcloo/data
          docker run --name="nginx" -it --volumes-from="nginx-data" -e VIRTUAL_HOST=example.com,www.example.com maxexcloo/nginx
	
  - **nginx + PHP-FPM**
	
          docker run --name="php-data" maxexcloo/data
          docker run --name="php" -it --volumes-from="php-data" -e VIRTUAL_HOST=example.com,www.example.com maxexcloo/nginx-php
	
  - **MariaDB** 
	
          docker run --name="mariadb-data" maxexcloo/data
          docker run --name="mariadb" -it --volumes-from="mariadb-data" maxexcloo/mariadb
	
  - **PostgreSQL**
	
          docker run --name="postgresql-data" maxexcloo/data
          docker run --name="postgresql" -it --volumes-from="postgresql-data" maxexcloo/postgresql
