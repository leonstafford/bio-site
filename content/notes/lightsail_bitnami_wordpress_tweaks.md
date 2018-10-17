+++
date = "2018-10-17"
title = "Tweaking the Bitnami WordPress Stack on AWS LightSail"
slug = "lightsail_bitnami_wordpress_tweaks"
tags = []
categories = []
+++


Tired of clunkily updating a GitHub gist, herein lie my notes used when
 provisioning an AWS LightSail instance with Bitnami WordPress for quick
 prototyping or debugging.

 -  copy public key via the virtual terminal in AWS Console

 -  enable SSL

https://metablogue.com/enable-lets-encrypt-ssl-aws-lightsail/

 - disable mod pagespeed

`sudo vim /opt/bitnami/apache2/conf/pagespeed.conf`




 - backup wp-config somewhere

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

 - rm bitnami banner
  ```
sudo /opt/bitnami/apps/wordpress/bnconfig --disable_banner 1
sudo /opt/bitnami/ctlscript.sh restart apache
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
 




[back](/)
