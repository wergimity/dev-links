Postfix setup
=============

Install postfix with mysql

`sudo apt-get install postfix postfix-mysql`

Add mailname file to etc

`echo "HOSTNAME" > /etc/mailname`

Find postfix uid and gui

`id postfix`

Add to the end of file `/etc/postfix/main.cf` replacing `GUI` and `UID`
```
# Add this to the end of file
message_size_limit=20480000

virtual_alias_maps = mysql:/etc/postfix/mysql_virtual_alias_maps.cf
virtual_gid_maps = static:GID
virtual_mailbox_base = /home/vmail
virtual_mailbox_domains = mysql:/etc/postfix/mysql_virtual_domains_maps.cf
virtual_mailbox_maps = mysql:/etc/postfix/mysql_virtual_mailbox_maps.cf
virtual_minimum_uid = UID
virtual_transport = virtual
virtual_uid_maps = static:UID

broken_sasl_auth_clients = yes
smtpd_recipient_restrictions =
    permit_mynetworks,
    permit_sasl_authenticated,
    reject_non_fqdn_hostname,
    reject_non_fqdn_sender,
    reject_non_fqdn_recipient,
    reject_unauth_destination,
    reject_unauth_pipelining,
    reject_invalid_hostname
    reject_rbl_client zen.spamhaus.org
smtpd_sasl_auth_enable = yes
smtpd_sasl_local_domain = $myhostname
smtpd_sasl_security_options = noanonymous
```

Create `/etc/postfix/mysql_virtual_alias_maps.cf` file with contents and edit with mysql credentials
```
user = postfix
password = password
hosts = 127.0.0.1
#host = localhost
dbname = postfix
table = alias
select_field = goto
where_field = address
query = SELECT goto FROM alias WHERE address = '%s'
```

Create `/etc/postfix/mysql_virtual_domains_maps.cf` file with contents and edit with mysql credentials
```
user = root
password = 456852
hosts = 127.0.0.1
#hosts = localhost
dbname = postfix
table = domain
select_field = domain
where_field = domain
additional_conditions = and backupmx = '0' and active = '1'
```

Create `/etc/postfix/mysql_virtual_mailbox_maps.cf` file with contents and edit with mysql credentials
```
user = root
password = 456852
hosts = 127.0.0.1
#hosts = localhost
dbname = postfix
table = mailbox
select_field = maildir
where_field = username
```

## Setup postfix admin
Download postfixadmin from http://sourceforge.net/projects/postfixadmin/

Install php imap extension

```
sudo apt-get install php5-imap
sudo php5enmod imap
```


Create database and user
```
CREATE DATABASE postfix;
CREATE USER 'postfix'@'localhost' IDENTIFIED BY 'choose_a_password';
GRANT ALL PRIVILEGES ON `postfix` . * TO 'postfix'@'localhost';
```

Edit `config.inc.php` file

```
$CONF['configured'] = true;
...
$CONF['database_type'] = 'mysqli';
$CONF['database_host'] = 'localhost';
$CONF['database_user'] = 'postfix';
$CONF['database_password'] = 'postfixadmin';
$CONF['database_name'] = 'postfix';
```

Go to `http://host/setup.php` and get `setup password hash` and add it to `config.inc.php`
Go to `http://host/setup.php` and create admin user

###Thats it!
