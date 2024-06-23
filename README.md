### The Airbyte connection to create in Airflow:

- Connection id: airbyte
- Connection type: Airbyte
- Host: airbyte-server
- Port: 8001


### Preconfig to start all docker compose services

#### Setting up a PostgreSQL Database

link: https://airflow.apache.org/docs/apache-airflow/2.8.2/howto/set-up-database.html#setting-up-a-postgresql-database


Start only the database service
```bash
docker compose up db
docker exec -it airbyte-db bash
psql -U docker
```

Create database, role and assign privileges
```sql
CREATE DATABASE airflow;
CREATE USER airflow WITH PASSWORD 'airflow';
GRANT ALL PRIVILEGES ON DATABASE airflow TO airflow;
```

```sql
psql -U docker airflow
GRANT ALL ON SCHEMA public TO airflow_user;
ALTER USER airflow SET search_path = public;
```

Verificate
```bash
\du # Show roles
\l # Show databases
```

#### Modify this envaironment variable into docker-compose file
```bash
AIRFLOW__DATABASE__SQL_ALCHEMY_CONN=postgresql+psycopg2://airflow:airflow@db/airflow
```

#### Up all services
```bash
docker compose down db
docker compose up
```

#### To access into mysql
```bash
docker exec -it airbyte_project-mysql-1 /bin/bash
mysql --user=docker --password data
SHOW TABLES;
```