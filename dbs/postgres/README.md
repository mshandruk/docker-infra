# postgres

Deploying PostgreSQL with Docker compose.

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

1. Install the postgres console client:

   ```bash
   apt install -y pgcli
   ```

2. Connect to postgres SQL console:

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

1. Create a configuration file:

   ```bash
   touch config/conf.d/custom.conf
   ```

2. Add custom parameters:

   Use the [PgTune generator](https://pgtune.fariton.ru) to generate performance tuning settings.

   ```text
   # DB Version: 16
   # OS Type: linux
   # DB Type: mixed
   # Total Memory (RAM): 4 GB
   # CPUs num: 2
   # Data Storage: ssd

   max_connections = 100
   shared_buffers = 1GB
   effective_cache_size = 3GB
   maintenance_work_mem = 256MB
   checkpoint_completion_target = 0.9
   wal_buffers = 16MB
   default_statistics_target = 100
   random_page_cost = 1.1
   effective_io_concurrency = 200
   work_mem = 2621kB
   huge_pages = off
   min_wal_size = 1GB
   max_wal_size = 4GB
   ```

3. Set Docker container limits in .env

   ```text
   # Container limits
   POSTGRES_CPU_LIMIT=2
   POSTGRES_MEMORY_LIMIT=4G
   POSTGRES_MEMORY_RESERVE=4G
   ```

4. Apply settings:

   ```bash
   docker compose up -d --build
   ```

5. Validate settings:

   ```bash
   docker compose exec -it db psql -U postgres -c "show shared_buffers;"
   ```
