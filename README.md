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

30 03 * * * critical_db_backup.sh
```
You can integrate this script with Zabbix by changing the above line to 

```
30 03 * * * zabbify.sh critical_db_backup.sh
```

THAT'S ALL!!!
====

zabbify will
* create an item `exit_status[critical_db_backup.sh]` on Zabbix and send the exit status of the script
* create an item `output[critical_db_backup.sh]` on Zabbix and send the output of the script
* create a trigger `{servername:exit_status[critical_db_backup.sh].last(0)}#0` to go off when the script exits with a non `0` status

More information in the wiki page

Please test it and report problems and suggestion to me
j