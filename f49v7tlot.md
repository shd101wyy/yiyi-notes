---
tags:
    - backend
    - backend/postgresql
id: ""
created: 2020-03-21T11:12:16.805Z
modified: 2020-03-21T11:38:45.153Z
---
# PostgreSQL Data Migration on Ubuntu 18.04

Article: [How To Move a PostgreSQL Data Directory to a New Location on Ubuntu 16.04](https://www.digitalocean.com/community/tutorials/how-to-move-a-postgresql-data-directory-to-a-new-location-on-ubuntu-16-04)

:point_up_2: Just tried and also worked on Ubuntu 18.04

1. Moving the PostgreSQL Data Directory

```bash
$ sudo -u postgres psql
```

Once you’ve entered the monitor, select the data directory:

```bash
postgres=# SHOW data_directory;
```

```
Output
       data_directory       
------------------------------
/var/lib/postgresql/10/main
(1 row)
```

2. Stop PostgreSQL

```bash
$ sudo systemctl stop psotgresql
```

3. Copy the old postgresql data storage to new location. For example:

```bash
$ sudo mkdir /mnt/postgresql/
$ sudo rsync -av /var/lib/postgresql/ /mnt/postgresql/
```

Backup the old one in case our migration failed

```bash
$ sudo mv /var/lib/postgresql/10/main /var/lib/postgresql/10/main.bak
```

4. Edit the PostgreSQL configuration to point to the new data location

```bash
$ sudo vim /etc/postgresql/10/main/postgresql.conf
```

Find the line that begins with `data_directory` and change the path which follows to reflect the new location.

In our case, the updated file looks like the output below:

```
...
data_directory = '/mnt/postgresql/10/main'
...
```

5. Restarting PostgreSQL

```bash
$ sudo systemctl start postgresql
$ sudo systemctl status postgresql
```

Repeat step `1` to check if the data location has been changed to the new one.  

6. Remove the backup and restart postgresql

```bash
$ sudo rm -rf /var/lib/postgresql/10/main.bak
$ sudo systemctl restart postgresql
$ sudo systemctl status postgresql
```