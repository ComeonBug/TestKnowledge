

# pg主从异常

1、如何确认主从异常了

2、如何恢复



```sql
# connect
psql -U postgres -h <PG_HOST> -p <PG_PORT>

# create pg db: workspace, visualization, alert, telemetry_dataops, airflow, meter, billing_agent, logstream_auth
create database <DB_NAME>;
```

