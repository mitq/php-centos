`yum update`

`yum install epel-release`

## Nginx

`yum install nginx`

`systemctl start nginx`

`systemctl enable nginx`

## MariaDB 10

`vi /etc/yum.repos.d/MariaDB.repo`

add

```
[mariadb]
name = MariaDB
baseurl = http://yum.mariadb.org/10.1/centos7-amd64
gpgkey=https://yum.mariadb.org/RPM-GPG-KEY-MariaDB
gpgcheck=1
```
then

`yum install MariaDB-server MariaDB-client`

`systemctl start mariadb`
Y, Y, Y, Y
`mysql_secure_installation`

`systemctl enable mariadb`

## PHP 7

add the Webtatic repo:

`rpm -Uvh https://mirror.webtatic.com/yum/el7/webtatic-release.rpm`

install

``yum install php70w``

see if it works or not

``php -v``

![](http://i.imgur.com/XFFuwvv.png)

Search available modules

`yum search php70`

![](http://i.imgur.com/ro6XviD.png)

install modules you need

```
yum install php70w-xml php70w-soap php70w-xmlrpc php70w-mbstring php70w-json php70w-gd php70w-mcrypt php70w-mysql 
yum install php70w-intl php70w-tidy
yum install php70w-pecl-redis 
yum install php-pecl-mongodb
yum install php70w-fpm
yum install php70w-devel php70w-pear
yum install php70w-pecl-apcu php70w-opcache
```
