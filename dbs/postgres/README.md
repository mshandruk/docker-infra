# Postgresql

1. Create a local `.env` configuration file:

```bash
cp .env.example .env
```

- Change value `POSTGRES_PASSWORD` and other settings.

2. Run the container:

```bash
docker compose up -d --build
```

3. Create user and database

```bash
docker compose exec -it db psql -U postgres
```

```bash
CREATE ROLE <some_user> WITH LOGIN PASSWORD '<secret>';

CREATE DATABASE <database_name>
WITH ENCODING='UTF-8'
     LC_COLLATE='en_US.UTF-8'
     LC_CTYPE='en_US.UTF-8'
     OWNER <some_user>;
```

## Example usage

### SQL console

1. Install postgres console client:

```bash
apt install -y pgcli
```

2. Connect to postgres server:

```bash
pgcli -h localhost -p 5432 -U <some_user> -d <database_name>
```

### Application

```yaml
services:
  web-app:
    image: web-app-image:latest
    environment:
      DATABASE_URL: "postgresql://<some_user>:<password>@db:5432/<app_db>"
    networks:
      - shared-infra-network

networks:
  shared-infra-network:
    external: true
```

## Custom configuration

The parameters in `config/postgresql.conf` must be overridden:

1. Create configuration file:

```bash
touch config/conf.d/custom.conf
```

2. Add custom parameters:

```text
max_connections = 150
work_mem = 4MB
```

3. Apply configuration:

```bash
docker compose restart db
```

Validate:

```bash
docker compose exec -it db psql -U postgres -c "show max_connections;"
```

[PgTune](https://pgtune.fariton.ru)
