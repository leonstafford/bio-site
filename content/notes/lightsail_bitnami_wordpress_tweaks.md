+++
date = "2018-10-17"
title = "Tweaking the Bitnami WordPress Stack on AWS LightSail"
slug = "lightsail_bitnami_wordpress_tweaks"
tags = []
categories = []
+++


Tired of clunkily updating a GitHub gist, herein lie my notes used when
 provisioning an AWS LightSail instance with Bitnami WordPress for quick
 migration/debugging of an existing user's WordPress site.

 - copy public key via the virtual terminal in AWS Console
 - set a DNS entry to point dev domain to the instance's IP
 - access site at dev domain
 - rm bitnami banner
  - ` sudo /opt/bitnami/apps/wordpress/bnconfig --disable_banner 1`
  - `sudo /opt/bitnami/ctlscript.sh restart apache`
 - SSH in using dev domain
 -  [enable SSL](https://metablogue.com/enable-lets-encrypt-ssl-aws-lightsail/)
  - `sudo mkdir /opt/bitnami/letsencrypt`
  - `cd /opt/bitnami/letsencrypt`
  - `sudo wget https://dl.eff.org/certbot-auto`
  - `sudo chmod a+x ./certbot-auto`
  - `sudo ./certbot-auto`
  - `sudo ./certbot-auto certonly --webroot -w /opt/bitnami/apps/wordpress/htdocs/ -d example.com`
  - `sudo ln -s /etc/letsencrypt/live/[DOMAIN]/fullchain.pem /opt/bitnami/apache2/conf/server.crt`
  - `sudo ln -s /etc/letsencrypt/live/[DOMAIN]/privkey.pem /opt/bitnami/apache2/conf/server.key`
  - Make sure that the certificate file name and path is correct. If you receive an error that file already exists, use the below command to rename the files:
  - `mv /opt/bitnami/apache2/conf/server.key /opt/bitnami/apache2/conf/serverkey.old`
  - `mv /opt/bitnami/apache2/conf/server.crt /opt/bitnami/apache2/conf/servercrt.old`
  - `sudo /opt/bitnami/ctlscript.sh restart apache`
 - adjust wp-config to use https
  - `sed -i 's/http/https/' /opt/bitnami/apps/wordpress/htdocs/wp-config.php`
  - `sudo chown -R bitnami:daemon /opt/bitnami/apps/wordpress/htdocs/`
 - force https in top of file: `/opt/bitnami/apps/wordpress/conf/httpd-prefix.conf`
  - `RewriteEngine On`
  - `RewriteCond %{HTTPS} !=on`
  - `RewriteRule ^/(.*) https://%{SERVER_NAME}/$1 [R,L]`
 - `sudo /opt/bitnami/ctlscript.sh restart apache`






 - backup wp-config somewhere


 - disable mod pagespeed

`sudo vim /opt/bitnami/apache2/conf/pagespeed.conf`





 - copy files into WP dir

 - replace wp-config
 - replace site urls in wp-config
 - rewrite any hard links within site
`find -name '*.css' -exec sed -i 's/OLD.domain.com/NEW.wp2static.com/g' {} +`
`wp search-replace 'OLD.domain.com' 'NEW.wp2static.com'`

 - reset file permissions

```
sudo chown -R bitnami:daemon /opt/bitnami/apps/wordpress/htdocs/
sudo find /opt/bitnami/apps/wordpress/htdocs/ -type f -exec chmod 664 {} \;
sudo find /opt/bitnami/apps/wordpress/htdocs/ -type d -exec chmod 775 {} \;
```


 - enable debugging

`define( 'WP_DEBUG', true );` 

**tailing WP logs**


`tail -f /opt/bitnami/apache2/logs/error_log`

**connect to db**

`mysql -u bn_wordpress -p bitnami_wordpress # and pwd from wp-config` 

**check plugin options**

`select * from wp_options where option_name = 'wp-static-html-output-options' \G;`

* `working_directory` option can persist the old sever's paths

**modify PHP ini**
# not yet needed, seems to have large `max_execution_time` already

 - disable opcache
 - disable mod pagespeed
 - use development level error logging

`sudo vim opt/bitnami/php/etc/php.ini`

**Basic Auth**

https://docs.bitnami.com/google/apps/wordpress-multisite/administration/use-htpasswd/
 

**less noisy error log**

`sudo echo 'ErrorLogFormat "\n \"%M\" \n  "' >> /opt/bitnami/apache2/conf/httpd.conf`

^ needed re-rediting to affect the CR's, look to fix


**import vim and tmux confs**

`curl https://gist.githubusercontent.com/leonstafford/6891b76aeaf43766dd52676d6bed1b08/raw/9979e28a9abf07af94b505d35ff9a4eb3da7cb9f/.tmux.conf --output ~/.tmux.conf`

`curl https://gist.githubusercontent.com/leonstafford/39333da3399adee7e88cb869b4685dff/raw/5e6e7b7fb834d996763da3d358d949d309691350/.vimrc --output ~/.vimrc`


[back](/)
