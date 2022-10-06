# Useful postgres Commands

### Reload config for Postgres

Login into database using **postgres**

```
psql
```

Then run,

```
SELECT pg_reload_conf();
```

This is when modifying the **pg_hba.conf** file and you want to apply changes. This won't cause an outage.

