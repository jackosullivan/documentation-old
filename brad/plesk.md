### MySQL
#### Access MySQL
```
plesk db
```
#### Get All Email Addresses & Passwords From Plesk and Output to File
```
mysql -uadmin -s -p `cat /etc/psa/.psa.shadow` -Dpsa -e"SELECT CONCAT_WS('@',mail.mail_name,domains.name),accounts.password FROM domains,mail,accounts WHERE domains.id=mail.dom_id AND accounts.id=mail.account_id ORDER BY domains.name ASC,mail.mail_name ASC;" > emailPassOutput.txt
```
#### Get All Email Addresses From Plesk and Output to File
```
mysql -uadmin -s -p `cat /etc/psa/.psa.shadow` -Dpsa -e"SELECT CONCAT_WS('@',mail.mail_name,domains.name) FROM domains,mail,accounts WHERE domains.id=mail.dom_id AND accounts.id=mail.account_id ORDER BY domains.name ASC,mail.mail_name ASC;" > emailOutput.txt
cut -d "@" -f 2 emailOutput.txt  | sort | uniq -c | sort -gr | less
```
#### Check MySQL Status
```
mysqladmin status -uadmin -s -p`cat/etc/psa/.psa.shadow`
```
### TLS
#### Disable v1.0 and v1.1
```
plesk bin server_pref -u -ssl-protocols 'TLSv1.2'
plesk sbin sslmng --protocols="TLSv1.2"
```
#### Check Current Ciphers (for backup purposes)
```
grep SSLCipher /etc/httpd/conf.d/ssl.conf
```
This should have an output similar to this:
```
#SSLCipherSuite ALL:!ADH:!EXPORT:!SSLv2:RC4+RSA:+HIGH:+MEDIUM:+LOW
SSLCipherSuite HIGH:!aNULL:!MD5
```
#### Change /etc/httpd/conf.d/ssl.conf SSLCipherSuite to
```
#SSLCipherSuite ALL:!ADH:!EXPORT:!SSLv2:RC4+RSA:+HIGH:+MEDIUM:+LOW
SSLCipherSuite ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-CHACHA20-POLY1305:ECDHE-RSA-CHACHA20-POLY1305:ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES256-SHA384:ECDHE-RSA-AES256-SHA384:ECDHE-ECDSA-AES128-SHA256:ECDHE-RSA-AES128-SHA256
```
