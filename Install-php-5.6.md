Repo設置
先確認您的伺服器已經打開SSH通道，然後執行下面指令(您需要有執行指令的權限，如果有必要建議使用root操作，或是sudo):

在CentOS 7下(包含安裝EPEL)
wget http://dl.fedoraproject.org/pub/epel/ ... el-release-7-8.noarch.rpm
wget http://rpms.famillecollet.com/enterprise/remi-release-7.rpm
rpm -Uvh remi-release-7*.rpm epel-release-7*.rpm
如果您原本就已經裝過EPEL則執行下面指令：

wget http://rpms.famillecollet.com/enterprise/remi-release-7.rpm
rpm -Uvh remi-release-7*.rpm
在CentOS 6下(包含安裝EPEL)
wget http://dl.fedoraproject.org/pub/epel/ ... el-release-6-8.noarch.rpm
wget http://rpms.famillecollet.com/enterprise/remi-release-6.rpm
rpm -Uvh remi-release-6*.rpm epel-release-6*.rpm
如果您原本就已經裝過EPEL則執行下面指令：

wget http://rpms.famillecollet.com/enterprise/remi-release-6.rpm
rpm -Uvh remi-release-6*.rpm
啟動程式庫(Repo,repository)
進行到目前為止，我們需要確認程式庫有被啟動，同時選訂我們想要安裝的版本。 We need to head over to /etc/yum.repos.d you should inside see a file called remi.repo.

使用您喜好的編輯器(Nano、Pico、Vi ...etc)開啟remi.repo這隻檔案，您將看到一部份參數。我們需要確認第一階段[remi]是啟用狀態。

[remi]
name=Les RPM de remi pour Enterprise Linux 6 - $basearch
#baseurl=http://rpms.famillecollet.com/enterprise/6/remi/$basearch/
mirrorlist=http://rpms.famillecollet.com/enterprise/6/remi/mirror
enabled=1
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-remi
這一行enabled=1確認要設定！在技術上現在是可以繼續進行PHP安裝，但是將只會取得PHP 5.4.X版本。這如果不是你要的，那麼跳到下一步驟！

If we want PHP 5.5 or PHP 5.6 we need to do a bit more work, further down in the repo.repo file you will see two additional sections [remi-php55] and [remi-php56], decide which PHP version you want to install and then enable the correct. So for PHP 5.6 we would change to:

[remi-php56]
name=Les RPM de remi de PHP 5.6 pour Enterprise Linux 6 - $basearch
#baseurl=http://rpms.famillecollet.com/enterprise/6/php56/$basearch/
mirrorlist=http://rpms.famillecollet.com/enterprise/6/php56/mirror
# WARNING: If you enable this repository, you must also enable "remi"
enabled=1
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-remi
Once you made your changes save your modified file and quit your editor.

Installing PHP
Now I’m assuming you don’t already have PHP installed, this bit is super simple.

sudo yum install php php-gd php-mysql php-mcrypt
So the above assumes you want MySQL, GD and Mcrypt support in your PHP, but you should see something like the below depending on which version of PHP you are trying to install:

================================================================================================================================
 Package                        Arch                   Version                                 Repository                  Size
================================================================================================================================
Installing:
 php                            x86_64                 5.5.20-2.el6.remi                       remi-php55                 2.6 M
 php-gd                         x86_64                 5.5.20-2.el6.remi                       remi-php55                  72 k
 php-mysqlnd                    x86_64                 5.5.20-2.el6.remi                       remi-php55                 3.6 M
Installing for dependencies:
 php-cli                        x86_64                 5.5.20-2.el6.remi                       remi-php55                 3.7 M
 php-common                     x86_64                 5.5.20-2.el6.remi                       remi-php55                 1.0 M
 php-pdo                        x86_64                 5.5.20-2.el6.remi                       remi-php55                 112 k
 php-pear                       noarch                 1:1.9.5-3.el6.remi                      remi                       375 k
 php-pecl-jsonc                 x86_64                 1.3.6-1.el6.remi.5.5.1                  remi-php55                  47 k
 php-pecl-zip                   x86_64                 1.12.4-1.el6.remi.5.5                   remi-php55                 269 k
 php-process                    x86_64                 5.5.20-2.el6.remi                       remi-php55                  57 k
 php-xml                        x86_64                 5.5.20-2.el6.remi                       remi-php55                 208 k

Transaction Summary
================================================================================================================================
Install      11 Package(s)
As you can see PHP is installing version 5.5.20-2.el6.remi from the remi-php55 repo! Once you have hit Y to confirm the install restart apache and magical unicorns you have a better version of PHP!

You can also change your mind in the future by going back into the remi.repo file and enable a different PHP version and then run yum update and if you have moved from 5.5 to 5.6 it will upgrade PHP for you. If you want to downgrade for any reason you will need to remove PHP (sudo yum remove php*) and then reinstall the PHP modules you want.

相關VI編輯器操作筆記
在putty下輸入
vi /etc/yum.repos.d/remi.repo
會打開/etc/yum.repos.d/remi.repo檔
此時是處於 c-mode 模式下，無法輸入文字，按「a」鍵，轉為 i-mode模式，即可開始進行修改，同時可以看到左下角提示「INSERT」，表示現在正在 i-mode 可修改模式下。

改完按「Esc」鍵，由i-mode修改模式回到c-mode指令模式，接著按冒號「:」進入命令列模式，此時左下角會出現冒號與閃爍游標，此時可以在冒號後輸入以下指令：
w: 存檔（write）。注意在編輯過程中所有內容只存在暫存器裡，必須在 c-mode 下了這個「:w」指令才會存檔。

e: 重新編輯（edit）。

q: 退出（quit），如果檔案經過修改而沒有存檔，會出現錯誤訊息：鍵入「:q!」強制退出 （此次作的修改會流失）