# Plesk
Plesk is a control panel commonly used for shared hosting platforms. 

## Plesk CLI
The Plesk CLI is a set of command-line tools which can be used to manage Plesk without using the web interface. It is much more in-depth than what I shall cover here, however I have included a small number of useful tools from the CLI below which are useful when investigating and resolving issues with Plesk servers.

### Accessing Plesk web-interface using CLI
These are the two commands you will likely need if you don't have the admin password for Plesk. The first command is for Plesk versions up to 12.5 and will give you the admin password. The second command is for Plesk 17 and up, and will give you a one-time login link.

#### Prior to Onyx
```/usr/local/psa/bin/admin --show-password```
#### Onyx
```plesk login```

### Accessing MySQL using CLI
If you don't have the admin MySQL password, then depending on your version of Plesk, you can retrieve / access MySQL using the CLI. This is only possible via the Plesk CLI on Onyx, but previous versions of Plesk also have a method of accessing MySQL which I have also documented here.

#### Prior to Onyx
```mysql -uadmin -p`cat /etc/psa/.psa.shadow` ```
#### Onyx
```plesk db```
