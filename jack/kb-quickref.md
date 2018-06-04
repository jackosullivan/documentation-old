### Useful Git Links
[Atlassian Git](https://www.atlassian.com/git/): Atlassian has some pretty incredible documentation on the basics of version control, git, and workflows. It's also got interactive tutorials on git for those that want to get used to it before working on the UKFast repos.

[Github command-line cheatsheet](https://services.github.com/on-demand/downloads/github-git-cheat-sheet.pdf): This is a really useful PDF cheatsheet on command-line git that helps with configuring git, and doing basic git processes.

[Git-Flow](http://nvie.com/posts/a-successful-git-branching-model/): This covers the git-flow workflow. It's pretty useful and I would suggest that everyone uses it until such a time that I actually publish some documentation on gitflow that relates better to our processes.

### WordPress
#### Check WordPress versions:
```
for i in $(locate version.php | grep wp-includes); do grep -H "wp_version = " ${i}; done | grep -v $(curl -s http://api.wordpress.org/core/version-check/1.1/ | tail -n1)
```
#### 500 Errors:
http://www.wpbeginner.com/wp-tutorials/how-to-fix-the-internal-server-error-in-wordpress/

https://www.elegantthemes.com/blog/tips-tricks/how-to-fix-the-500-internal-server-error-on-your-wordpress-website

#### Security:
https://codex.wordpress.org/Hardening_WordPress

https://wpvulndb.com/

#### Minimum / Recommended Requirements:
https://wordpress.org/about/requirements/

### Crons
#### Add the following to the end of a cron to prevent emails being sent out by the cron:
 ```
 >/dev/null 2>&1 
 ```
#### List all of the Cronjobs for each user:
```
for user in $(cut -f1 -d: /etc/passwd); do echo $user && crontab -u $user -l; done
```
### MySQL
#### List users and their respective host:
```
SELECT User,Host FROM mysql.user;
```
#### Starting a Second MySQL Instance:
```
/usr/sbin/mysqld --socket=/tmp/mysql2.sock --datadir={restore_location} --skip-networking --pid-file=/tmp/mysql2.pid --user=mysql
```
You should add ```innodb_force_recovery={1-6}``` if the second instance will not start.

### PHP Mail
#### Mail test one-liner
```
php -r "var_dump(mail('jack.osullivan@ukfast.co.uk', 'test subject', 'test body'));"
```

### Plesk
#### Check the running version of Plesk:
```
cat /usr/local/psa/version
```
#### Get the admin password:
```
/usr/local/psa/bin/admin --show-password
```
#### Get mail passwords:
```
/usr/local/psa/admin/sbin/mail_auth_view
```
### NMap Stuff
#### Test SSL Ciphers
```
nmap -sV --script ssl-enum-ciphers -p 443 <host>
```
### File Limits
#### Symptom
500 error being thrown in the browser, (24: Too many open files) in the logs.

#### Solution
Just follow the following guide: https://gist.github.com/joewiz/4c39c9d061cf608cb62b

### Disabling SSL Stapling
#### Apache
```
SSLUseStapling off
```
If the server you are applying this change to is a cPanel server, then you will need to specify the stapling off line in the following file: ```/usr/local/apache/conf/includes/pre_virtualhost_global.conf```

#### Nginx
comment out the following lines:
```
ssl_stapling on;
ssl_stapling_verify on;
ssl_trusted_certificate /etc/nginx/ssl/full_chain.pem;
```
### Apache: No space left on device.
https://major.io/2007/08/24/apache-no-space-left-on-device-couldnt-create-accept-lock/
