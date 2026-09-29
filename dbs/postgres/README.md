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
