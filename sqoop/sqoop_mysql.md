
# Setting a Mysql Data base  

## Installing a Mysql Data base  
```sh
yum localinstall https://dev.mysql.com/get/mysql57-community-release-el7-9.noarch.rpm

yum install mysql-community-server

systemctl enable mysqld.service

```  

## Reducing the passowrd latency  
```sh
edit the /etc/my.cnf file and add just this one line:

validate_password_policy=LOW
```  

## Setting a root password for mysql database
```sh
grep 'A temporary password is generated for root@localhost' /var/log/mysqld.log |tail -1

/usr/bin/mysql_secure_installation

mysqladmin -u root password myownsecrectpass

mysql -u root -p
```

## Providing all privilages on a data base
```sh
**first create a database and create a few tables in it
**every table must have a primary key
grant all privileges on Database_name.* to 'root'@'localhost';
FLUSH PRIVILEGES;
```

## Installing jdbc connector for mysql to sqoop connection
```sh
yum install mysql-connector-java
```

