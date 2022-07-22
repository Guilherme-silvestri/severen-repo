version: '3.5'
services:
 mysql-server:
  image: mysql:8.0
  networks:
   - zbx_net
  command:
   - mysqld
   - --character-set-server=utf8
   - --collation-server=utf8_bin
   - --default-authentication-plugin=mysql_native_password
  environment:
   - MYSQL_USER=zabbix
   - MYSQL_DATABASE=zabbixdb
   - MYSQL_PASSWORD=PasswOrd
   - MYSQL_ROOT_PASSWORD=StrongPassword
   - ZBX_JAVAGATEWAY=zabbix-java-gateway
  volumes:
   - /volumes/zabbix-mysql:/var/lib/mysql:rw
 zabbix-server-mysql:
  image: zabbix/zabbix-server-mysql:alpine-latest
  networks:
   - zbx_net
  ports:
   - 10051:10051
  volumes:
   - /etc/localtime:/etc/localtime:ro
   - /etc/timezone:/etc/timezone:ro 
   - /volumes/zabbix-data/alertscripts:/usr/lib/zabbix/alertscripts:ro
   - /volumes/zabbix-data/externalscripts:/usr/lib/zabbix/externalscripts:ro
   - /volumes/zabbix-data/export:/var/lib/zabbix/export:rw
   - /volumes/zabbix-data/modules:/var/lib/zabbix/modules:ro
   - /volumes/zabbix-data/enc:/var/lib/zabbix/enc:ro
   - /volumes/zabbix-data/ssh_keys:/var/lib/zabbix/ssh_keys:ro
   - /volumes/zabbix-data/mibs:/var/lib/zabbix/mibs:ro
   - /volumes/zabbix-data/snmptraps:/var/lib/zabbix/snmptraps:rw
  environment:
   - DB_SERVER_HOST=mysql-server
   - MYSQL_DATABASE=zabbixdb
   - MYSQL_USER=zabbix
   - MYSQL_PASSWORD=Passw0rd
   - MYSQL_ROOT_PASSWORD=StrongPassword
   - ZBX_JAVAGATEWAY=zabbix-java-gateway

  depends_on:
   - mysql-server

 zabbix-web-nginx-mysql:
  image: zabbix/zabbix-web-nginx-mysql:alpine-6.0-latest
  networks:
   - zbx_net
  ports:
   - 80:8080
   - 443:8443
  volumes:
   - /etc/localtime:/etc/localtime:ro
   - /etc/timezone:/etc/timezone:ro
   - /volumes/zabbix-nginx/nginx:/etc/ssl/nginx:ro
   - /volumes/zabbix-nginx/modules/:/usr/share/zabbix/modules/:ro
  environment:
   - ZBX_SERVER_HOST=zabbix-server-mysql
   - DB_SERVER_HOST=mysql-server
   - MYSQL_DATABASE=zabbixdb
   - MYSQL_USER=zabbix
   - MYSQL_PASSWORD=Passw0rd
   - MYSQL_ROOT_PASSWORD=StrongPassword
  depends_on:
   - mysql-server
   - zabbix-server-mysql
 zabbix-java-gateway:
  image: zabbix/zabbix-java-gateway:alpine-6.0-latest
  networks:
   - zbx_net
  ports:
   - 10052:10052
networks:
 zbx_net:
   driver: bridge