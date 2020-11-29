# WordPress 5.x deployment guide for Arm-based AWS Graviton2 processors

This document explains the process of installation, configuration and hardening of Apache server, based on Amazon Linux 2 default installation (Arm-based AWS Graviton2 processors)



## Web server installation phase

1. Login to the EC2 console:

   https://console.aws.amazon.com/ec2/v2/home

2. From the upper right pane, choose the target Region

3. From the left pane, under Instances -> Instances -> Launch instances

4. On the "Choose an Amazon Machine Image (AMI)" page, select "Amazon Linux 2 AMI (HVM), SSD Volume Type", with 64-Bit (Arm) architecture

5. On the "Choose an Instance Type" page, select "t4g.medium" -> click Next: Configure Instance Details

6. On the "Configure Instance Details" page:

   * Advanced Details -> User data -> paste the following text into the empty field:

     `#!/bin/bash && 
     sudo su &&
     yum update -y &&
     yum install httpd mod_ssl -y &&
     chown root:root /usr/sbin/apachectl &&
     chown root:root /usr/sbin/httpd &&
     chmod 770 /usr/sbin/apachectl &&
     chmod 770 /usr/sbin/httpd &&
     chown -R root:root /etc/httpd &&
     chmod -R go-r /etc/httpd &&
     chown -R root:root /etc/httpd/logs &&
     chmod -R 700 /etc/httpd/logs &&
     amazon-linux-extras install -y lamp-mariadb10.2-php7.2 php7.2 &&
     mkdir /etc/httpd/sites-available &&
     mkdir /etc/httpd/sites-enabled &&
     mkdir -p /www &&
     chown -R apache:apache /www &&
     chmod 2775 /www && find /var/www -type d -exec sudo chmod 2775 {} \; &&
     yum install -y mariadb-server &&
     systemctl start httpd.service &&
     systemctl enable httpd.service &&
     systemctl start mariadb &&
     systemctl enable mariadb`

7. Click Next: Add Storage -> Leave default settings
8. Click Next: Add Tags
   * Key: Name
   * Value: Specify here the hostname for the web server (such as ***WordPress01\***)

9. Click Next: Configure Security Group
10. On the "Configure Security Group" page:
    * Replace Security group name to: ***Web-SecurityGroup\***
    * Replace Description to: ***Web-SecurityGroup\***
    * On the SSH protocol rule, change the Source IP to your organization public IP
11. Click Add Rule and add the following rules:
    * Type: ***HTTP\***, Source: ***0.0.0.0/0, ::/0\*** , Description: ***Allow_HTTP\***
    * Type: ***HTTPS\***, Source: ***0.0.0.0/0, ::/0\*** , Description: ***Allow_HTTPS\***
12. Click Review and Launch -> Click Launch
13. On the "Select an existing key pair or create a new key pair" page, choose "Create a new key pair" -> specify the target key pair name -> click Download Key Pair
14. Click Launch Instances -> click View Instances



## Securing the database server

1. Login to the web server via SSH

2. Run the command bellow to set ownership and permissions for /etc/my.cnf file:

   `sudo chown root /etc/my.cnf
   sudo chmod 644 /etc/my.cnf`

3. Edit using VI, the file /etc/my.cnf and add the string bellow under the [mysqld] section

   `bind-address = 127.0.0.1`

4. Run the command below to secure the MySQL:

   `sudo mysql_secure_installation`

5. Specify the MySQL root account password (leave blank) -> Press Y to set the Root password -> specify new complex password (at least 14 characters, upper case, lower case, number, special characters) and document it -> Press Y to remove anonymous users -> Press Y to disallow root login remotely -> Press Y to remove test database -> Press Y to reload privilege tables and exit the script

6. Restart the MariaDB service:

   `sudo systemctl restart mariadb.service`



## PHP 7.x configuration phase

1. Login to the web server via SSH

2. Change the permissions on the php.ini file:

   `sudo chmod 640 /etc/php.ini`

3. Edit using VI, the file /etc/php.ini

| Original string/section | **New  string/section**                                      |
| ----------------------- | ------------------------------------------------------------ |
| mysqli.default_host  =  | mysqli.default_host  = 127.0.0.1:3306                        |
| allow_url_fopen  = On   | allow_url_fopen  = Off                                       |
| expose_php  = On        | expose_php  = Off                                            |
| memory_limit  = 128M    | memory_limit  = 8M                                           |
| post_max_size  = 8M     | post_max_size  = 2M                                          |
| disable_functions  =    | disable_functions  = fpassthru,crack_check,crack_closedict,crack_getlastmessage,crack_opendict,  psockopen,php_ini_scanned_files,shell_exec,chown,hell-exec,dl,ctrl_dir,phpini,tmp,safe_mode,systemroot,server_software,  get_current_user,HTTP_HOST,ini_restore,popen,pclose,exec,suExec,passthru,proc_open,proc_nice,proc_terminate,  proc_get_status,proc_close,pfsockopen,leak,apache_child_terminate,posix_kill,posix_mkfifo,posix_setpgid,  posix_setsid,posix_setuid,escapeshellcmd,escapeshellarg,posix_ctermid,posix_getcwd,posix_getegid,posix_geteuid,posix_getgid,posix_getgrgid,  posix_getgrnam,posix_getgroups,posix_getlogin,posix_getpgid,posix_getpgrp,posix_getpid,  posix_getppid,posix_getpwnam,posix_getpwuid,posix_getrlimit,system,posix_getsid,posix_getuid,posix_isatty,  posix_setegid,posix_seteuid,posix_setgid,posix_times,posix_ttyname,posix_uname,posix_access,posix_get_last_error,posix_mknod,  posix_strerror,posix_initgroups,posix_setsidposix_setuid |

4. Restart the Apache service:

   `sudo systemctl restart httpd.service`



## WordPress 5.x installation phase

1. Login to the web server via SSH

2. Run the command bellow to login to the MariaDB:

   `sudo /usr/bin/mysql -uroot -p`

   Note: When prompted, specify the password for the MariaDB root account.

3. Run the following commands from the MariaDB prompt:

   `CREATE USER 'blgusr'@'localhost' IDENTIFIED BY 'A3fg1j7x!s2gEq';
   CREATE DATABASE m6gf42s;
   GRANT ALL PRIVILEGES ON m6gf42s.* TO "blgusr"@"localhost" IDENTIFIED BY "A3fg1j7x!s2gEq";
   FLUSH PRIVILEGES;
   quit`

   Note 1: Replace “blgusr” with a username to access first the database

   Note 2: Replace “A3fg1j7x!s2gEq” with complex password for the account who will access the first database (at least 14 characters, upper case, lower case, number, special characters)

   Note 3: Replace “m6gf42s” with the first WordPress database name

4. Run the commands below to download the latest build of WordPress:

   `cd /usr/local/src
   sudo wget https://wordpress.org/latest.zip
   sudo unzip latest.zip -d /www`

5. Create using VI the file ***/www/config.php\*** with the following content:

   `<?php
   define('DB_NAME', 'm6gf42s');
   define('DB_USER', 'blgusr');
   define('DB_PASSWORD', 'A3fg1j7x!s2gEq');
   define('DB_HOST', 'localhost');
   $table_prefix  = 'm6gf42s_';
   define('AUTH_KEY', 'put your unique phrase here');
   define('SECURE_AUTH_KEY', 'put your unique phrase here');
   define('LOGGED_IN_KEY', 'put your unique phrase here');
   define('NONCE_KEY', 'put your unique phrase here');
   define('AUTH_SALT',        'put your unique phrase here');
   define('SECURE_AUTH_SALT', 'put your unique phrase here');
   define('LOGGED_IN_SALT',   'put your unique phrase here');
   define('NONCE_SALT',       'put your unique phrase here');
   define('FS_METHOD', 'direct');
   ?>`

   Note 1: Make sure there are no spaces, newlines, or other strings before an opening '< ?php' tag or after a closing '?>' tag

   Note 2: Replace “blgusr” with MariaDB account to access the first database

   Note 3: Replace “A3fg1j7x!s2gEq” with complex password (at least 14 characters)

   Note 4: Replace “m6gf42s” with the first WordPress database name

   Note 5: In-order to generate random values for the AUTH_KEY, SECURE_AUTH_KEY, LOGGED_IN_KEY and NONCE_KEY, use the web site bellow:

   http://api.wordpress.org/secret-key/1.1/

6. Copy the ***wp-config.php\*** file:

   `sudo cp /www/wordpress/wp-config-sample.php /www/wordpress/wp-config.php`

7. Edit using VI, the file ***/www/wordpress/wp-config.php\***

   * Add the following lines before the string “**That's all, stop editing! Happy publishing**”:

     `/* Multisite */
     define('WP_ALLOW_MULTISITE', true);
     include('/www/config.php');`

   * Remove or comment the following sections:

     `define('DB_NAME', 'database_name_here');
     define('DB_USER', 'username_here');
     define('DB_PASSWORD', 'password_here');
     define('DB_HOST', 'localhost');
     $table_prefix  = 'wp_';
     define('AUTH_KEY', 'put your unique phrase here');
     define('SECURE_AUTH_KEY', 'put your unique phrase here');
     define('LOGGED_IN_KEY', 'put your unique phrase here');
     define('NONCE_KEY', 'put your unique phrase here');
     define('AUTH_SALT',        'put your unique phrase here');
     define('SECURE_AUTH_SALT', 'put your unique phrase here');
     define('LOGGED_IN_SALT',   'put your unique phrase here');
     define('NONCE_SALT',       'put your unique phrase here');`

8. Create using VI the file ***/www/wordpress/.htaccess\*** and add the following content:

   `# BEGIN WordPress
   <IfModule mod_rewrite.c>
   RewriteEngine On
   RewriteBase /
   RewriteRule ^index\.php$ - [L]
   RewriteCond %{REQUEST_FILENAME} !-f
   RewriteCond %{REQUEST_FILENAME} !-d
   RewriteRule . /index.php [L]
   </IfModule>
   \# END WordPress
   Header set X-XSS-Protection "1; mode=block"
   Header set X-Content-Type-Options nosniff
   Header set Content-Security-Policy "default-src 'self' 'unsafe-inline' 'unsafe-eval' https: data:"`

   Note: Remove the character \ before the string "# End WordPress"

9. Create using VI the file ***/www/wordpress/wp-content/.htaccess\*** and add the following content:

   `# BEGIN WordPress
   <IfModule mod_rewrite.c>
   RewriteEngine On
   RewriteBase /
   RewriteRule ^index\.php$ - [L]
   RewriteCond %{REQUEST_FILENAME} !-f
   RewriteCond %{REQUEST_FILENAME} !-d
   RewriteRule . /index.php [L]
   </IfModule>
   \# END WordPress`

   Note: Remove the character \ before the string "# End WordPress"

10. Create using VI the file ***/www/wordpress/wp-includes/.htaccess\*** and add the following content:

    `# BEGIN WordPress
    <IfModule mod_rewrite.c>
    RewriteEngine On
    RewriteBase /
    RewriteRule ^index\.php$ - [L]
    RewriteCond %{REQUEST_FILENAME} !-f
    RewriteCond %{REQUEST_FILENAME} !-d
    RewriteRule . /index.php [L]
    </IfModule>
    \# END WordPress`

    Note: Remove the character \ before the string "# End WordPress"

11. Set ownership and permissions on the **.htaccess** files below:

    `sudo chown apache:apache /www/wordpress/.htaccess
    sudo chown apache:apache /www/wordpress/wp-content/.htaccess
    sudo chown apache:apache /www/wordpress/wp-includes/.htaccess
    sudo chmod 644 /www/wordpress/.htaccess
    sudo chmod 644 /www/wordpress/wp-content/.htaccess
    sudo chmod 644 /www/wordpress/wp-includes/.htaccess`

12. Remove default content from the first WordPress site:

    `sudo rm -f /www/wordpress/license.txt
    sudo rm -f /www/wordpress/readme.html
    sudo rm -f /www/wordpress/wp-config-sample.php
    sudo rm -f /www/wordpress/wp-content/plugins/hello.php`



## Apache Configuration Phase

1. Login to the web server via SSH

2. Edit using VI the file ***/etc/httpd/conf/httpd.conf\*** and change the following strings:

   From:

   `LogLevel warn`

   To:

   `LogLevel notice`



​		From:

​		`DocumentRoot "/var/www/html"`

​		To:

​		`# DocumentRoot "/var/www/html"`



​		From:

​		`ScriptAlias /cgi-bin/ "/var/www/cgi-bin/"`

​		To:

​		`# ScriptAlias /cgi-bin/ "/var/www/cgi-bin/"`

3. Comment out the entire sections below inside the ***/etc/httpd/conf/httpd.conf\***

   `<Directory />
   <Directory "/var/www">
   <Directory "/var/www/html">
   <Directory “/var/www/cgi-bin”>`

4. Add the following sections to the end of the ***/etc/httpd/conf/httpd.conf\*** file:

   `IncludeOptional sites-enabled/*.conf
   ServerTokens Prod
   ServerSignature Off
   TraceEnable Off
   LimitRequestBody 4000000
   LimitRequestFields 40
   LimitRequestFieldSize 4000
   LimitRequestLine 4000
   MaxRequestsPerChild 10000
   Header always append X-Frame-Options SAMEORIGIN`

5. Remove the files below:

   `sudo mv /etc/httpd/conf.d/autoindex.conf /etc/httpd/conf.d/autoindex.conf.bak
   sudo mv /etc/httpd/conf.d/userdir.conf /etc/httpd/conf.d/userdir.conf.bak`

6. Comment out the lines inside the ***/etc/httpd/conf.modules.d/00-base.conf\*** file below to disable default modules:

   `LoadModule status_module modules/mod_status.so
   LoadModule info_module modules/mod_info.so
   LoadModule autoindex_module modules/mod_autoindex.so
   LoadModule include_module modules/mod_include.so
   LoadModule userdir_module modules/mod_userdir.so
   LoadModule env_module modules/mod_env.so
   LoadModule negotiation_module modules/mod_negotiation.so
   LoadModule actions_module modules/mod_actions.so`

7. Comment out the lines inside the ***/etc/httpd/conf.modules.d/01-cgi.conf\*** file below to disable default modules:

   `LoadModule cgi_module modules/mod_cgi.so`

8. Using VI, create configuration file for the first WordPress site called ***/etc/httpd/sites-available/websitea.com.conf\*** with the following content:

   `<VirtualHost *:80>
       ServerAdmin admin@websitea.com
       ServerName www.websitea.com
       ServerAlias websitea.com
       DocumentRoot /www/wordpress
       <Directory />
           Options FollowSymLinks
           AllowOverride None
       </Directory>
       <Directory /www/wordpress>
           Options Indexes FollowSymLinks MultiViews
           AllowOverride all
           Require all granted
           Order allow,deny
           Allow from all
           <LimitExcept GET POST>
               deny from all
           </limitexcept>
       </Directory>
   </VirtualHost>`

   Note: Replace WebSiteA.com with the relevant name

9. Run the commands below to enable the new virtual host files:

   `sudo ln -s /etc/httpd/sites-available/websitea.com.conf /etc/httpd/sites-enabled/websitea.com.conf`

   Note: Replace WebSiteA with the relevant name

10. Restart the Apache service:

    `sudo systemctl restart httpd.service`

11. Open a web browser from a client machine, and enter the URL bellow:

    [http://**Server_FQDN**/wp-admin/install.php](http://Server_FQDN/wp-admin/install.php)

    Note: Replace Server_FQDN with the relevant DNS name

12. Select language and click Continue

13. Specify the following information:

    * Site Title
    * Username - replace the default "admin"
    * Password
    * E-mail

14. Click on “Install WordPress” button, and close the web browser

15. From WordPress dashboard, click on "settings" -> make sure that "Anyone can register" is left unchecked -> put a new value inside the "Tagline" field -> click on "Save changes"

16. From the upper pane, click on "Log Out"

17. Change ownership and permissions on the files and folders below:

    `sudo chown -R apache:apache /www/wordpress
    sudo find /www/wordpress/ -type d -exec chmod -R 755 {} \;
    sudo find /www/wordpress/ -type f -exec chmod -R 644 {} \;
    sudo chmod 400 /www/wordpress/wp-config.php 
    sudo chown apache:apache /www/config.php
    sudo chmod 644 /www/config.php`

18. Delete the file /wp-admin/install.php

    `sudo rm -f /www/wordpress/wp-admin/install.php`



## SSL/TLS Configuration Phase

1. Login to the web server via SSH

2. Run the command below to change the permissions on the certificates folder:

   `sudo chmod 700 /etc/pki/CA/private`

3. Run the command bellow to generate a key pair for the first WordPress site:

   `sudo openssl genrsa -des3 -out /etc/pki/CA/private/server.key 2048`

   Note: Specify a complex pass phrase for the private key (and document it)

4. Run the command bellow to generate the CSR for the first WordPress site:

   `sudo openssl req -new -newkey rsa:2048 -nodes -sha256 -keyout /etc/pki/CA/private/server.key -out /tmp/apache.csr`

   Note: The command above should be written as one line

5. Edit using VI the file ***/etc/httpd/sites-available/websitea.com.conf\*** and add the following:

   `<VirtualHost *:443>
       ServerAdmin admin@websitea.com
       ServerName www.websitea.com
       ServerAlias websitea.com
       DocumentRoot /www/wordpress
       <Directory />
           Options FollowSymLinks
           AllowOverride None
       </Directory>
       <Directory /www/wordpress>
           Options Indexes FollowSymLinks MultiViews
           AllowOverride all
           Require all granted
           Order allow,deny
           Allow from all
           <LimitExcept GET POST>
               deny from all
           </limitexcept>
       </Directory>
       SSLCertificateFile /etc/ssl/certs/websitea.com.crt
       SSLCertificateKeyFile /etc/pki/CA/private/server.key
       SSLCipherSuite EECDH+ECDSA+AESGCM:EECDH+aRSA+AESGCM:EECDH+ECDSA+SHA384:EECDH+ECDSA+SHA256:EECDH+aRSA+SHA384:EECDH+aRSA+SHA256:ECDHE-ECDSA-AES256-SHA:ECDHE-RSA-AES128-SHA256:AES128-GCM-SHA256:ECDH+AESGCM:DH+AESGCM:ECDH+AES256:DH+AES256:DH+AES:ECDH+3DES:DH+3DES:RSA+AES:RSA+3DES:!ADH:!AECDH:!MD5:!DSS:!aNULL:!EDH:!eNULL:!LOW:!3DES:!MD5:!EXP:!PSK:!SRP:!DSS
       SSLHonorCipherOrder On
       SSLProtocol ALL -SSLv2 –SSLv3 +TLSv1 +TLSv1.1 +TLSv1.2
       SSLCompression Off
       SSLEngine on
   </VirtualHost>`

   Note: Replace WebSiteA.com with the relevant name

6. Edit using VI the file ***/etc/httpd/conf.d/ssl.conf\*** and comment the following commands:

   `<VirtualHost _default_:443>
   ErrorLog logs/ssl_error_log
   TransferLog logs/ssl_access_log
   LogLevel warn
   SSLEngine on
   SSLProtocol all -SSLv3
   SSLCipherSuite HIGH:MEDIUM:!aNULL:!MD5:!SEED:!IDEA
   SSLCertificateFile
   SSLCertificateKeyFile`

7. Restart the Apace service, run the command below:

   `sudo systemctl restart httpd`

8. Run the command below to change the permissions on the certificates folder:

   `sudo chmod 600 /etc/pki/CA/private`

9. In-case the server was configured with SSL certificate, add the following line to the ***/www/config.php\*** file:

   `define('FORCE_SSL_LOGIN', true);`



## Amazon Linux Patch Upgrade Process

1. Login to the web server via SSH

2. Run the command below:

   `sudo yum update -y`

3. Remove orphan files:

   `sudo rm /etc/httpd/conf.d/autoindex.conf
   sudo rm /etc/httpd/conf.d/userdir.conf`

