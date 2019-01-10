# Plesk
Plesk is a control panel commonly used for shared hosting platforms. 

| Contents                                                                                                              |
|-----------------------------------------------------------------------------------------------------------------------|
|[**Plesk CLI**](https://docs.osullivan.sh/plesk/#plesk-cli)                                                            |
|[*Accessing Plesk web-interface using CLI*](https://docs.osullivan.sh/plesk/#accessing-plesk-web-interface-using-cli)  |
|[*Accessing MySQL using CLI*](https://docs.osullivan.sh/plesk/#accessing-mysql-using-cli)                              |
|[*Dumping databases using CLI*](https://docs.osullivan.sh/plesk/#dumping-databases-using-cli)                          |
|[**FTP**](https://docs.osullivan.sh/plesk/#ftp)                                                                        |
|[*Configuring Passive FTP*](https://docs.osullivan.sh/plesk/#passive-ftp)                                              |

## Plesk CLI
The Plesk CLI is a set of command-line tools which can be used to manage Plesk without using the web interface. It is much more in-depth than what I shall cover here, however I have included a small number of useful tools from the CLI below which are useful when investigating and resolving issues with Plesk servers.

### Accessing Plesk web-interface using CLI
These are the two commands you will likely need if you don't have the admin password for Plesk. The first command is for Plesk versions up to 12.5 and will give you the admin password. The second command is for Plesk 17 and up, and will give you a one-time login link.

#### Prior to Onyx
```
/usr/local/psa/bin/admin --show-password
```

#### Onyx
```
plesk login
```

### Accessing MySQL using CLI
If you don't have the admin MySQL password, then depending on your version of Plesk, you can retrieve / access MySQL using the CLI. This is only possible via the Plesk CLI on Onyx, but previous versions of Plesk also have a method of accessing MySQL which I have also documented here.

#### Prior to Onyx
```
mysql -uadmin -p`cat /etc/psa/.psa.shadow` 
```

#### Onyx
```
plesk db
```

### Dumping databases using CLI
The Plesk db CLI also has the facility to dump databases to file. It is somewhat more straight forward than using the mysqldump command and have to cat the shadow file out which is why I have included it here. As is the case with large parts of the Plesk CLI, I am unsure as to when this was introduced and so I have also included the old method.

#### Old Method (mysqldump)
```
mysqldump -uadmin -p`cat /etc/psa.psa.shadow` [database_name]
```

#### New Method (Plesk CLI)
```
plesk db dump [database_name]
```

## FTP
One of the core functions of Plesk is the ability to use and manage your website via FTP. Here is some information you may find useful about FTP on Plesk.

### Passive FTP
I have written a seperate guide on setting up Passive FTP when running on a NATed server called [*Configuring Passive FTP*](https://docs.osullivan.sh/plesk/Passive-FTP).
