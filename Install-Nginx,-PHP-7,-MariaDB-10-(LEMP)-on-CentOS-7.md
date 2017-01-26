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

`vi /etc/php.ini`

find `cgi.fix_pathinfo=1` and then replace it with `cgi.fix_pathinfo=0`

`vi /etc/php-fpm.d/www.conf`

find and replace with:

1)
```
listen = /var/run/php-fpm/php-fpm.sock
```
2)
```
listen.owner = nobody
listen.group = nobody
```
3)
```
user = nginx
group = nginx
```
Start php-fpm
`systemctl start php-fpm`
Enable it to boot
`systemctl enable php-fpm`

`vi /etc/nginx/conf.d/default.conf`
```
server {
    listen       80;
    server_name  localhost;

    # note that these lines are originally from the "location /" block
    root   /usr/share/nginx/html;
    index index.php index.html index.htm;

    location / {
        try_files $uri $uri/ =404;
    }
    error_page 404 /404.html;
    error_page 500 502 503 504 /50x.html;
    location = /50x.html {
        root /usr/share/nginx/html;
    }

    location ~ \.php$ {
        try_files $uri =404;
        fastcgi_pass unix:/var/run/php-fpm/php-fpm.sock;
        fastcgi_index index.php;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        include fastcgi_params;
    }
}
```