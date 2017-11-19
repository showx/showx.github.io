---
layout: post
title: 在Mac OS X中配置Apache ＋ PHP ＋ MySQL
date: 2013-07-31 19:56
author: admin
comments: true
categories: []
---
Mac OS X 内置Apache 和 PHP，使用起来非常方便。本文以Mac OS X 10.6.3和<ins datetime="2012-08-30T14:34:02+08:00"> 10.8.1</ins>为例。主要内容包括：
<ol>
	<li><a href="http://dancewithnet.com/2010/05/09/run-apache-php-mysql-in-mac-os-x/#apache">启动Apache</a></li>
	<li><a href="http://dancewithnet.com/2010/05/09/run-apache-php-mysql-in-mac-os-x/#php">运行PHP</a></li>
	<li><a href="http://dancewithnet.com/2010/05/09/run-apache-php-mysql-in-mac-os-x/#mysql">安装MySQL</a></li>
	<li><a href="http://dancewithnet.com/2010/05/09/run-apache-php-mysql-in-mac-os-x/#phpmyadmin">使用phpMyAdmin</a></li>
	<li><a href="http://dancewithnet.com/2010/05/09/run-apache-php-mysql-in-mac-os-x/#mcrypt">配置PHP的MCrypt扩展库</a></li>
	<li><a href="http://dancewithnet.com/2010/05/09/run-apache-php-mysql-in-mac-os-x/#virtualhost">设置虚拟主机</a></li>
</ol>
<h3 id="apache">启动Apache</h3>
有两种方法：
<ol>
	<li>打开“系统设置偏好（System Preferences）” -&gt; “共享（Sharing）” -&gt; “Web共享（Web Sharing）”。<ins datetime="2012-08-30T14:34:02+08:00">注意，从Mac OS X从10.8开始取消了 “Web共享（Web Sharing）”。</ins></li>
	<li>打开“终端（terminal）”，然后（注意，sudo需要的密码就是系统的root帐号密码）
<ol>
	<li>运行“<code>sudo apachectl start</code>”，再输入帐号密码，这样Apache就运行了。</li>
	<li>运行“<code>sudo apachectl －v</code>”，你会看到Mac OS X的Apache版本信息，如10.8.1中：
<pre><code>Server version: Apache/2.2.22 (Unix)
Server built:   Jun 20 2012 13:57:09
</code></pre>
</li>
</ol>
</li>
</ol>
如此在浏览器中输入“http://localhost”，就可以看到一个内容为“It works!”的页面，其位于“/Library（资源库）/WebServer/Documents/”下，这就是Apache的默认根目录。

&nbsp;

注意：开启了Apache就是开启了“Web共享”，这时联网用户就会通过“http://[本地IP]/”来访问“/Library（资源库）/WebServer/Documents/”目录，通过“http://[本地IP]/~[用户名]”来访问“/Users/[用户名]/Sites/”目录。<ins datetime="2012-08-30T14:34:02+08:00">值得注意的是，Mac OS X在10.8中取消”Web共享（Web Sharing）”时，也移除了“/Users/[用户名]/Sites/”目录，所以10.8中访问“http://[本地IP]/~[用户名]”会显示“403 Forbidden”，但http://[本地IP]/依旧可以访问。</ins>可以到“系统偏好设置” -&gt; “安全（Security）” -&gt; “防火墙（Firewall）”，开启防火墙，然后在“防火墙选项（Firewall Options）”中勾上“组织所有进入连接（block all incoming connections）”即可。也可以通过设置httpd.conf来只允许localhost和127.0.0.1访问“/Library（资源库）/WebServer/Documents/”。
<pre><code>&lt;Directory "/Library/WebServer/Documents"&gt;
    ......
    #
    # Controls who can get stuff from this server.
    #
    Order allow,deny
    #Allow from all
    Allow from 127.0.0.1
    Allow from localhost 

&lt;/Directory&gt;
</code></pre>
<h3 id="php">运行PHP</h3>
<ol>
	<li>在终端中运行“<code>sudo vi /etc/apache2/httpd.conf</code>”，打开Apache的配置文件。（如果不习惯操作终端和vi，可以设置<a href="http://apple.tgbus.com/tutorial/soft/200811/20081125100105.shtml">在Finder中显示所有系统隐藏文件</a>，记得设置完毕后需要<a href="http://www.macdocks.com/2009/07/14/mac-tips-%E4%B8%89%E7%A7%8D%E6%96%B9%E6%B3%95%E9%87%8D%E5%90%AFmac-os-x-finder/">重启Finder</a>，然后就可以找到对应文件，随心所欲编辑了，需要注意的是某些文件的修改还是需要<a href="http://support.apple.com/kb/HT1528?viewlocale=zh_CN">开启root帐号</a>，但整体上还是在终端上使用<code>sudo</code>来临时获取root权限比较安全。）</li>
	<li>找到“<code>#LoadModule php5_module libexec/apache2/libphp5.so</code>”，把前面的#号去掉，保存（在命令行输入<code>:w</code>）并退出vi（在命令行输入<code>:q</code>）。</li>
	<li>运行“<code>sudo cp /etc/php.ini.default /etc/php.ini</code>”，这样就可以运行<code>sudo vi /etc/php.ini</code>来编辑php.ini配置各种功能了。比如：
<pre><code>;通过下面两项来调整PHP提交文件的最大值，如phpMyAdmin中导入数据的最大值
upload_max_filesize = 2M
post_max_size = 8M
;通过display_errors来控制是否显示PHP程序的报错信息，这在调试PHP程序时非常有用
display_errors = Off
</code></pre>
</li>
	<li>运行“<code>sudo apachectl restart</code>”，重启Apache，这样PHP就可以用了。</li>
	<li>运行“<code>sudo cp /Library/WebServer/Documents/index.html.en /Library/WebServer/Documents/info.php</code>”，即在Apache的根目录下复制index.html.en文件并重命名为info.php。</li>
	<li>在终端中运行“<code>sudo vi /Library/WebServer/Document/info.php</code>”，这样就可以在vi中编辑info.php文件了。在“It’s works!”后面加上“<code>&lt;?php phpinfo(); ?&gt;</code>”，然后保存之。如此就可以在http://localhost/info.php中看到有关PHP的信息，比如10.8中内置PHP版本号是5.3.13。</li>
</ol>
<h3 id="mysql">安装MySQL</h3>
Mac OS X没有内置MySQL，所以需要自己手动安装，目前MySQL的最稳定版本是5.5。<a href="http://dev.mysql.com/doc/refman/5.5/en/macosx-installation.html">MySQL提供了Mac OS X下的安装说明</a>。
<ol>
	<li><a href="http://dev.mysql.com/downloads/mysql/5.5.html">下载MySQL 5.5</a>。选择合适版本，如这里选择了mysql-5.5.27-osx10.6-x86_64.dmg。</li>
	<li>运行dmg，会发现里面有4个文件。首先点击安装mysql-5.5.27-osx10.6-x86_64.pkg，这是MySQL主安装包。一般情况下，安装文件会自动把MySQL安装到<code>/usr/local</code>下的同名文件夹下。如运行“<code>mysql-5.5.27-osx10.6-x86_64.dmg</code>”会把MySQL安装到“<code>/usr/local/mysql-5.5.27-osx10.6-x86_64</code>”中，一路默认安装完毕。（注意，从10.8开始Mac OS X的权限更加严格，直接点击会提示“mysql-5.5.27-osx10.6-x86_64.pkg can’t be opened because it is from an unidentified developer. Your security preferences allow installation of only apps from the Mac App Store and identified developers.”阻止了安装，你可以使用双指单击该安装文件，在弹出菜单中选择“用…打开（open with）”，再选择“安装（Installer）”就可以接着安装了。）</li>
	<li>安装第2个文件MySQLStartupItem.pkg，MySQL就会自动在开机时启动了。（注意，10.8的安装方法同上。）</li>
	<li>安装第3个文件MySQL.prefPane，就会在“系统设置偏好”中看到“MySQL”的ICON，通过它就可以控制MySQL是否开启，以及开机时是否自动运行。到这里MySQL就基本安装完毕了。（注意，10.8中用双指单击该安装文件，在弹出的菜单中选择“用…打开（open with）”，然后选择“系统偏好（System Perference）”就可以接着安装了。）</li>
	<li>通过运行“<code>sudo vi /etc/bashrc</code>”，在bash配置文件中加入<code>mysqlstart</code>、<code>mysql</code>和<code>mysqladmin</code>的别名（注意：修改完毕之后需要退出“终端（Terminal）”之后重新进入，这些命令才会生效）：
<pre><code>#mysql
alias mysqlstart='sudo /Library/StartupItems/MySQLCOM/MySQLCOM restart'
alias mysql='/usr/local/mysql/bin/mysql'
alias mysqladmin='/usr/local/mysql/bin/mysqladmin'
</code></pre>
这样就可以在终端中比较简单地通过命令进行相应的操作。由于开始安装MySQLStartupItem.pkg到“<code>/Library/StartupItems/MySQLCOM/</code>”来控制MySQL的运行、自动运行、停止、关闭之类。在MySQL没有启动时，直接运行<code>mysql</code>或<code>mysqladmin</code>命令会提示“<code>Can't connect to local MySQL server through socket '/tmp/mysql.sock' (2)</code>”，所以我们可以通过控制面板或者直接运行<code>mysqlstart</code>命令来启动MySQL，之后再运行<code>mysql</code>或<code>mysqladmin</code>命令就正常了。比如安装完毕后MySQL的<code>root</code>默认密码为空，如果要设置密码可以在终端运行“<code>mysqladmin -u root password "mysqlpassword"</code>”来设置，其中mysqlpassword即root的密码。更多相关内容可以参考<a href="http://dev.mysql.com/doc/refman/5.5/en/resetting-permissions.html">B.5.4.1. How to Reset the Root Password</a>。</li>
</ol>
注意：Mac OS X的升级或其他原因可能会导致ＭySQL启动或开机自动运行时，在ＭySQL操作面板上会提示“<code>Warning:The /usr/local/mysql/data directory is not owned by the 'mysql' or '_mysql' </code>”，这应该是某种情况下导致<code>/usr/local/mysql/data</code>的宿主发生了改变，只需要运行“<code>sudo chown -R mysql /usr/local/mysql/data</code>”即可。

另外，<a href="http://www.exp2up.com/2009/03/11/mac%E4%B8%8A%E7%9A%84php%E6%97%A0%E6%B3%95%E8%BF%9E%E6%8E%A5mysql/">使用PHP连接MySQL可能会报错</a>“Can’t connect to local MySQL server through socket ‘/var/mysql/mysql.sock’”，或使用localhost无法连接MySQL而需要127.0.0.1，原因是连接时php默认去找<code>/var/mysql/mysql.sock</code>了，但MAC版的MYSQL改动了文件位置，放在/tmp下了。处理办法是按如下修改php.ini：
<pre><code>mysql.default_socket = /tmp/mysql.sock</code></pre>
<h3 id="phpmyadmin">使用phpMyAdmin</h3>
<a href="http://www.phpmyadmin.net/">phpMyAdmin</a>是用PHP开发的管理MySQL的程序，非常的流行和实用。能够使用phpMyAdmin管理MySQL是检验前面几步效果的非常有效方式。
<ol>
	<li><a href="http://www.phpmyadmin.net/home_page/downloads.php">下载phpMyAdmin</a>。选择合适的版本，比如这里选择phpMyAdmin-3.5.22-all-languages.tar.bz2这个版本。</li>
	<li>把“下载（downloads）”中phpMyAdmin-3.5.22-all-languages.tar.bz2文件解压到“<code>/Library/WebServer/Documents/</code>”中，并改名为phpmyadmin。
<pre><code>sudo tar -xf ~/Downloads/phpMyAdmin-3.5.2.2-all-languages.tar.bz2 -C
             /Library/WebServer/Documents/
sudo mv /Library/WebServer/Documents/phpMyAdmin-3.5.2.2-all-languages
            /Library/WebServer/Documents/phpmyadmin
</code></pre>
</li>
	<li>复制“<code>/Library/WebServer/Documents/phpmyadmin/</code>”中的config.sample.inc.php，并命名为config.inc.php</li>
	<li>编辑config.inc.php，修改如下：
<pre><code>用于Cookie加密，随意的长字符串
$cfg['blowfish_secret'] = 'a8b7c6d';

当phpMyAdmin中出现“#2002 无法登录 MySQL 服务器（#2002 Cannot log in to the MySQL server）”时，
<a href="http://achan.me/2010/04/mysql-error-2002.html">请把localhost改成127.0.0.1就ok了</a>，
这是因为MySQL守护程序做了IP绑定（bind-address =127.0.0.1）造成的
$cfg['Servers'][$i]['host'] = 'localhost';

把false改成true，这样就可以访问无密码的MySQL了，
即使MySQL设置了密码也可以这样设置，然后登录phpMyAdmin时输入密码
$cfg['Servers'][$i]['AllowNoPassword'] = false;
</code></pre>
</li>
	<li>这样就可以通过<code>http://localhost/phpmyadmin</code>访问phpMyAdmin了。此时会看到一个提示“无法加载 mcrypt 扩展，请检查您的 PHP 配置。（The mcrypt extension is missing. Please check your PHP configuration.）”，这会涉及到下一节安装MCrypt扩展了。</li>
</ol>
<h3 id="mcrypt">配置PHP的MCrypt扩展</h3>
<a href="http://mcrypt.sourceforge.net/">MCrypt</a>是一个功能强大的加密算法扩展库，它包括有22种算法，phpMyAdmin依赖这个PHP扩展库。但在Mac OS X下的安装却不那么友善，具体如下：
<ol>
	<li>下载<a href="http://sourceforge.net/projects/mcrypt/files/Libmcrypt/">libmcrypt-2.5.8.tar.gz</a>。</li>
	<li>在终端执行如下命令（注意如下命令需要安装<a href="http://developer.apple.com/xcode/">Xcode</a>支持，可直接去Mac App Store下载，安装完毕后可能会发现在终端运行<code>./configure --disable-posix-threads --enable-static</code>会报错，运行<code>make</code>会提示命令不存在，此时还需要打开Xcode，然后在Xcode的软件“配置（Preference…）”）-&gt; “下载（Downloads）” 中安装 “命令行工具（Command Line Tools）”：
<pre><code>cd ~/Downloads
tar -zxvf libmcrypt-2.5.8.tar.bz2
cd libmcrypt-2.5.8
./configure --disable-posix-threads --enable-static
make
sudo make install</code></pre>
</li>
	<li>下载<a href="http://us2.php.net/get/php-5.3.13.tar.bz2/from/a/mirror">PHP源码文件php-5.3.13.tar.bz2</a>，记得选择中国镜像会比较快。Mac OS X 10.6.3中预装的PHP版本是5.3.1，10.8的版本是5.3.13，而现在<a href="http://us2.php.net/downloads.php">最新的PHP版本是5.4.6</a>，所以需要依据自己的实际情况选择对应的版本，本文以10.8的PHP版本为例。</li>
	<li>在终端执行如下命令，把php-5.3.13.tar.bz2，并<a href="http://www.coolestguyplanettech.com/how-to-install-mcrypt-for-php-on-mac-osx-lion-10-7-development-server/">配置autoconf</a>（在新的Mac OS X的Xcode中需要自己配置），然后才能运行<code>phpize</code>命令：
<pre><code>cd ~/Downloads
tar -zxvf php-5.3.13.tar.bz2

cd php-5.3.13/ext/mcrypt
curl -O http://ftp.gnu.org/gnu/autoconf/autoconf-latest.tar.gz
tar -zxvf autoconf-latest.tar.gz
cd autoconf-2.69
./configure
make
sudo make install

cd ..
phpize
./configure
make
sudo make install
</code></pre>
</li>
	<li>打开php.ini
<pre><code>sudo vi /etc/php.ini</code></pre>
在php.ini中加入如下代码，并保存后退出，然后重启Apache
<pre><code>extension=mcrypt.so</code></pre>
</li>
</ol>
当你再访问<code>http://localhost/phpmyadmin</code>时，会发现“无法加载 mcrypt 扩展，请检查您的 PHP 配置。”提示没有了，这就表示MCrypt扩展库安装成功了。如果还不能加载，尝试把php.ini中的加入的<code>extension</code>修改为：
<pre><code>extension=/usr/lib/php/extensions/no-debug-non-zts-20090626/mcrypt.so</code></pre>
Mac OS X下安装MCrypt扩展的确比较复杂，而且稍微不小心会有各种小问题出现，大家还可以参考<a href="http://www.coolestguyplanettech.com/how-to-install-mcrypt-for-php-on-mac-osx-lion-10-7-development-server/">How to Install mcrypt for php on Mac OSX Lion 10.8 &amp; 10.7 Development Server</a>和<a href="http://remonpel.nl/2012/01/adding-mcrypt-to-your-osx-lion-php-install/">Adding MCRYPT to your OSX Lion PHP install</a>
<h3 id="virtualhost">设置虚拟主机</h3>
<ol>
	<li>在终端运行“<code>sudo vi /etc/apache2/httpd.conf</code>”，打开Apche的配置文件</li>
	<li>在httpd.conf中找到“<code>#Include /private/etc/apache2/extra/httpd-vhosts.conf</code>”，去掉前面的“<code>＃</code>”，保存并退出。</li>
	<li>运行“<code>sudo apachectl restart</code>”，重启Apache后就开启了虚拟主机配置功能。</li>
	<li>运行“<code>sudo vi /etc/apache2/extra/httpd-vhosts.conf</code>”，就打开了配置虚拟主机文件httpd-vhost.conf，配置虚拟主机了。需要注意的是该文件默认开启了两个作为例子的虚拟主机：
<pre><code>&lt;VirtualHost *:80&gt;
    ServerAdmin webmaster@dummy-host.example.com
    DocumentRoot "/usr/docs/dummy-host.example.com"
    ServerName dummy-host.example.com
    ErrorLog "/private/var/log/apache2/dummy-host.example.com-error_log"
    CustomLog "/private/var/log/apache2/dummy-host.example.com-access_log" common
&lt;/VirtualHost&gt;

&lt;VirtualHost *:80&gt;
    ServerAdmin webmaster@dummy-host2.example.com
    DocumentRoot "/usr/docs/dummy-host2.example.com"
    ServerName dummy-host2.example.com
    ErrorLog "/private/var/log/apache2/dummy-host2.example.com-error_log"
    CustomLog "/private/var/log/apache2/dummy-host2.example.com-access_log" common
&lt;/VirtualHost&gt; </code></pre>
而实际上，这两个虚拟主机是不存在的，在没有配置任何其他虚拟主机时，可能会导致访问localhost时出现如下提示：
<pre><code>Forbidden
You don't have permission to access /index.php on this server</code></pre>
最简单的办法就是在它们每行前面加上#，注释掉就好了，这样既能参考又不导致其他问题。</li>
	<li>增加如下配置
<pre><code>&lt;VirtualHost *:80&gt;
    DocumentRoot "/Library/WebServer/Documents"
    ServerName localhost
    ErrorLog "/private/var/log/apache2/localhost-error_log"
    CustomLog "/private/var/log/apache2/localhost-access_log" common
&lt;/VirtualHost&gt; 

&lt;VirtualHost *:80&gt;
    DocumentRoot "/Users/[用户名]/Sites"
    ServerName sites
    ErrorLog "/private/var/log/apache2/sites-error_log"
    CustomLog "/private/var/log/apache2/sites-access_log" common
    &lt;Directory /&gt;
                Options Indexes FollowSymLinks MultiViews
                AllowOverride None
                Order deny,allow
                Allow from all
      &lt;/Directory&gt;
&lt;/VirtualHost&gt; </code></pre>
保存退出，并重启Apache。</li>
	<li>运行“<code>sudo vi /etc/hosts</code>”，打开hosts配置文件，加入"<code>127.0.0.1 sites</code>"，这样就可以配置完成sites虚拟主机了，可以访问“http://sites”了，在10.8之前Mac OS X版本其内容和“http://localhost/~[用户名]”完全一致。</li>
	<li>注意，记录log的“<code>ErrorLog "/private/var/log/apache2/sites-error_log"</code>”也可以删掉，但记录日志其实是一个好习惯，在出现问题时可以帮助我们判断。如果保留这些log代码，一定log文件路径都是存在的，如果随便修改一个不存在的，会导致Apache无法服务而没有错误提示，这个比较恶心。</li>
</ol>
这里利用Mac OS X 10.6.3和10.8.1中原生支持的方式来实现的配置，也可以参考“<a href="http://blog.csdn.net/afatgoat/archive/2008/12/26/3615026.aspx">Mac OS X Leopard: 配置Apache, PHP, SQLite, MySQL, and phpMyAdmin(一) </a>”和“<a href="http://blog.csdn.net/afatgoat/archive/2008/12/28/3628710.aspx">Mac OS X Leopard: 配置Apache, PHP, SQLite, MySQL, and phpMyAdmin(二) </a>”。实际上，还可以使用<a href="http://www.apachefriends.org/en/xampp-macosx.html">XAMPP</a>或<a href="http://www.macports.org/">MacPorts</a>这种第三方提供的集成方案来实现简单的安装和使用。
