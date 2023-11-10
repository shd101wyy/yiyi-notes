## Backup

```bash
sudo su - postgres

pg_dump --dbname $DB --schema=$SCHEMA > backup.sql
```

## Restore

```bash
sudo su - postgres

psql -d $DB -U $USER -h localhost -f backup.sql
```

## Useful commands

```
\l - Display database
\c - Connect to database
\dn - List schemas
\dt - List tables inside public schemas
\dt schema1. - List tables inside particular schemas. For eg: 'schema1'.
```

```
$ psql

postgres=# \c ercx
postgres=# set schema 'pr-463'
ercx=#     SELECT * FROM "pr-463".user WHERE username = 'shd101wyy';
ercx=#     UPDATE "pr-463".user SET stripe_customer_id = NULL WHERE username = 'shd101wyy';
```