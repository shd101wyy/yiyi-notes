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