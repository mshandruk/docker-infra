# Postgresql

1. Create a local `.env` configuration file:

```bash
cp .env.example .env
```

And update the settings in .env.

2. Run

```bash
docker compose up -d --build
```

3. Create role

```bash
docker compose exec db psql -h localhost -U postgres
```

```bash
CREATE ROLE <some_user> WITH LOGIN PASSWORD '<secret>';
```

```bash

CREATE DATABASE <database_name>
WITH ENCODING='UTF-8'
     LC_COLLATE='<your_target_locale>.<your_target_encoding>'
     LC_CTYPE='<your_target_locale>.<your_target_encoding>'
     OWNER <some_user>;
```

## Example usage

### Pgcli

1. Install postgres client

```bash
apt install -y pgcli
```

2. Connect to postgres server

```bash
pgcli -h localhost -U <some_user> -d <database_name>
```

### Application server

```yaml
services:
  web-app:
    image: some-app-image:latest
    environment:
      DATABASE_URL: "postgresql://<some_user>:<db_password>@db:5432/<app_db>"
    networks:
      - shared-infra-network

networks:
  shared-infra-network:
    external: true
```
