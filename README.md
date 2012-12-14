zabbify
=======

a script to automatically integrate (crontab) scripts with Zabbix

Picture the tipical scenario when you have an infrastructure with many servers, with many scripts running 
with crontab and doing all sort of tasks at all time.

You probably keep an eye on them with thousands of cryptic emails that you tend to ignore until something bad happens.

I wrote this little script to help integrate these kind of scripts with Zabbix, so that you can remove these emails
and clean up your mailbox.

Examples
=======

```
MAILTO=admin@mycompany.com

00 05 * * * /usr/bin/mysqldump --databases zabbix --single-transaction | /bin/gzip > /opt/backups/mysql/zabbix/zabbix.sql.gz 2> /dev/null
```
You can integrate this script with Zabbix by changing the above line to 

```
00 05 * * * zabbify "/usr/bin/mysqldump --databases zabbix --single-transaction | /bin/gzip > /opt/backups/mysql/zabbix/zabbix.sql.gz 2> /dev/null"
```

THAT'S ALL!!!
====

zabbify will
* create an item `exit_status[mysqldump]` on Zabbix and send the exit status of the script
* create an item `output[mysqldump]` on Zabbix and send the output of the script
* create a trigger `{servername:exit_status[mysqldump].last(0)}#0` to go off when the script exits with a non `0` status

*IMPORTANT*: Zabbix Server needs a couple of minutes from when the items are created to when you can use them to send data to. Please run it a few times and then wait if you do not see them being populated.

INSTALLATION
====

* Create a config file in `/etc/zabbify.conf` like the one provided as example
* Install `zabbify` in a convenient location (`/usr/bin`) and give appropriate permissions (`chmod 555 /usr/bin/zabbify`)
* The script depends on the `zabbix_api` and `zbxsend` libraries. Please install them in the `PYTHONPATH`
* `zbxsend` will not work with Python3. The version I have included adds some minor fixes. Will send the patch to the author soon.

TEST
====

* Run `zabbify` with test scripts to see if it works. Good examples are:

``` 
zabbify /bin/true
zabbify /bin/false
zabbify echo test
zabbify --itemname true /bin/false
zabbify "cat somefile | grep something"
```

* If you have problems, change the error level in `/etc/zabbix.conf` to `DEBUG` and rerun


More information (soon) in the wiki page

Please test it and report problems and suggestion to me 
