`yum update`

`yum install epel-release`

`yum install nginx`

`systemctl start nginx`

`systemctl enable nginx`

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