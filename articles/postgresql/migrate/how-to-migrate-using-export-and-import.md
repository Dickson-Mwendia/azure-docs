---
title: Migrate a database - Azure Database for PostgreSQL - Single Server
description: Describes how extract a PostgreSQL database into a script file and import the data into the target database from that file.
ms.service: postgresql
ms.subservice: migration-guide
ms.topic: how-to
ms.author: srranga
author: sr-msft
ms.date: 09/22/2020
---
# Migrate your PostgreSQL database using export and import
[!INCLUDE[applies-to-postgres-single-flexible-server](../includes/applies-to-postgres-single-flexible-server.md)]

You can use [pg_dump](https://www.postgresql.org/docs/current/static/app-pgdump.html) to extract a PostgreSQL database into a script file and [psql](https://www.postgresql.org/docs/current/static/app-psql.html) to import the data into the target database from that file.

## Prerequisites
To step through this how-to guide, you need:
- An [Azure Database for PostgreSQL server](../single-server/quickstart-create-server-database-portal.md) with firewall rules to allow access and database under it.
- [pg_dump](https://www.postgresql.org/docs/current/static/app-pgdump.html) command-line utility installed
- [psql](https://www.postgresql.org/docs/current/static/app-psql.html) command-line utility installed

Follow these steps to export and import your PostgreSQL database.

## Create a script file using pg_dump that contains the data to be loaded
To export your existing PostgreSQL database on-premises or in a VM to a sql script file, run the following command in your existing environment:

```bash
pg_dump --host=<host> --username=<name> --dbname=<database name> --file=<database>.sql
```
For example, if you have a local server and a database called **testdb** in it:
```bash
pg_dump --host=localhost --username=masterlogin --dbname=testdb --file=testdb.sql
```

## Import the data on target Azure Database for PostgreSQL
You can use the psql command line and the --dbname parameter (-d) to import the data into the Azure Database for PostgreSQL server and load data from the sql file.

```bash
psql --file=<database>.sql --host=<server name> --port=5432 --username=<user> --dbname=<target database name>
```
This example uses psql utility and a script file named **testdb.sql** from previous step to import data into the database **mypgsqldb** on the target server **mydemoserver.postgres.database.azure.com**.

For **Single Server**, use this command 
```bash
psql --file=testdb.sql --host=mydemoserver.database.windows.net --port=5432 --username=mylogin@mydemoserver --dbname=mypgsqldb
```

For **Flexible Server**, use this command  
```bash
psql --file=testdb.sql --host=mydemoserver.database.windows.net --port=5432 --username=mylogin --dbname=mypgsqldb
```



## Next steps
- To migrate a PostgreSQL database using dump and restore, see [Migrate your PostgreSQL database using dump and restore](how-to-migrate-using-dump-and-restore.md).
- For more information about migrating databases to Azure Database for PostgreSQL, see the [Database Migration Guide](/data-migration/).
