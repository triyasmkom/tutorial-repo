# Restore and Backup database mysql
### Backup database from command line with mysqldump
```
sudo mysqldump -u [username] -p [database_name] > [filename].sql

sudo mysqldump -u root -p example_db > example_db.sql
```


### Restore database mysql
1. Create new database
    ```
    - login to mysql with CLI

        mysql -u root -p

    - create new database

        create database example_db
    ```
2. Restore from file .sql
    ```
    mysql -u root -p example_db < example_db.sql
    ```
Reference:
https://phoenixnap.com/kb/how-to-backup-restore-a-mysql-database


If your find this error:
```
#1273 – Unknown collation: ‘utf8mb4_unicode_520_ci’
```

Please your remove collation `utf8mb4_unicode_520_ci` in your file `example_db.sql`