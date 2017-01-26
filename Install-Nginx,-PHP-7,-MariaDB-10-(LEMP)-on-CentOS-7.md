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
`yum install php70w`
> Installed:
>   php70w.x86_64 0:7.0.14-1.w7

> Dependency Installed:
>   apr.x86_64 0:1.4.8-3.el7            apr-util.x86_64 0:1.5.2-6.el7
>   httpd.x86_64 0:2.4.6-45.el7.centos  httpd-tools.x86_64 0:2.4.6-45.el7.centos
>   mailcap.noarch 0:2.1.41-2.el7       php70w-cli.x86_64 0:7.0.14-1.w7
>   php70w-common.x86_64 0:7.0.14-1.w7

see if it works or not
`php -v`
Search available modules
yum search php70
![](http://i.imgur.com/ro6XviD.png)
install modules you need
yum install php70w-xml php70w-soap php70w-xmlrpc php70w-mbstring php70w-json php70w-gd php70w-mcrypt php70w-mysql