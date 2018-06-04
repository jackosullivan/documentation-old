## Site URL
### MySQL (CAREFUL)
Check the current siteurl as defined in MySQL
```
mysql -e "SELECT * FROM DatabaseName.wp_options;" | grep --color siteurl
```
Replace all mentions of the site url in MySQL
```use databaseName;
UPDATE wp_options SET option_value = replace(option_value, 'http://oldurl.com', 'http://newurl.com') WHERE option_name = 'home' OR option_name = 'siteurl';
UPDATE wp_posts SET guid = replace(guid, 'http://oldurl.com','http://newurl.com');
UPDATE wp_posts SET post_content = replace(post_content, 'http://oldurl.com', 'http://newurl.com');
UPDATE wp_postmeta SET meta_value = replace(meta_value,'http://oldurl.com', 'http://newurl.com');
```
#### wp-config.php
```
define('WP_HOME','http://oldurl.com');
define('WP_SITEURL','http://oldurl.com');
```
#### functions.php Theme File
Put the following lines after the `<?php` line
```
update_option( 'siteurl', 'http://oldurl.com' );
update_option( 'home', 'http://oldurl.com' );
```
Load the admin/login page a few times

### Get Admin: MySQL
```
INSERT INTO `wp_users` (user_login, user_pass, user_nicename, user_email, user_status) VALUES ('ukfastsupport', MD5('c7awraP5!'), 'UKFast Support', 'test@example.com', '0');
INSERT INTO `wp_usermeta` (umeta_id, user_id, meta_key, meta_value) VALUES (NULL, (Select max(id) FROM wp_users), 'wp_capabilities', 'a:1:{s:13:"administrator";s:1:"1";}');
INSERT INTO `wp_usermeta` (umeta_id, user_id, meta_key, meta_value) VALUES (NULL, (Select max(id) FROM wp_users), 'wp9_user_level', '10');
```
### Default .htaccess
```
# BEGIN WordPress
<IfModule mod_rewrite.c>
RewriteEngine On
RewriteBase /
RewriteRule ^index\.php$ - [L]
RewriteCond %{REQUEST_FILENAME} !-f
RewriteCond %{REQUEST_FILENAME} !-d
RewriteRule . /index.php [L]
</IfModule>
# END WordPress
```
### Nginx Additional Directives
```
if (!-e $request_filename) { 
set $test P; 
}
if ($uri !~ ^/(plesk-stat|webstat|webstat-ssl|ftpstat|anon_ftpstat|awstats-icon|internal-nginx-static-location)) { 
set $test "${test}C"; 
} 
if ($test = PC) { 
rewrite ^/(.*)$ /index.php?$1; 
}
```
### Fix Permissions/Ownership
#### Repair Permissions
Note: Ensure you are currently in the directory. This will alter file permissions via chmod.
```
pwd
find ./ -type d -exec chmod 755 {} \;
find ./ -type f -exec chmod 644 {} \;
chmod 660 ./wp-config.php
find ./wp-content/ -type d -exec chmod 775 {} \;
find ./wp-content/ -type f -exec chmod 664 {} \;
```
#### Repair Script
```
#!/bin/bash
#
# This script configures WordPress file permissions based on recommendations
# from http://codex.wordpress.org/Hardening_WordPress#File_permissions
#
#
WP_OWNER=usernameHere # <-- wordpress owner
WP_GROUP=usergroupHere # <-- wordpress group
WP_ROOT=$1 # <-- wordpress root directory
WS_GROUP=www-data # <-- webserver group (apache?)

# reset to safe defaults
find ${WP_ROOT} -exec chown ${WP_OWNER}:${WP_GROUP} {} \;
find ${WP_ROOT} -type d -exec chmod 755 {} \;
find ${WP_ROOT} -type f -exec chmod 644 {} \;

# allow wordpress to manage wp-config.php (but prevent world access)
chgrp ${WS_GROUP} ${WP_ROOT}/wp-config.php
chmod 660 ${WP_ROOT}/wp-config.php

# allow wordpress to manage wp-content
find ${WP_ROOT}/wp-content -exec chgrp ${WS_GROUP} {} \;
find ${WP_ROOT}/wp-content -type d -exec chmod 775 {} \;
find ${WP_ROOT}/wp-content -type f -exec chmod 664 {} \;
```
### Check WordPress Versions Running On Server
```
updatedb
for i in $(locate version.php | grep wp-includes); do grep -H "wp_version = " ${i}; done | grep -v $(curl -s http://api.wordpress.org/core/version-check/1.1/ | tail -n1)
```
