# Infrastructure Monitoring

A decentralized monitoring based on independent Docker Compose service stacks.

## Key Configuration Paths

- `prometheus/config/prometheus.yml` - Main Prometheus scrape target configuration file.
- `grafana/provisioning/datasources/datasources.yml` - Automated data source provisioning rules.
- `grafana/provisioning/dashboards/` - Preinstalled dashboards.

## Services Overview

| Service       | Access                                                 | Description                                            |
| :------------ | :----------------------------------------------------- | :----------------------------------------------------- |
| prometheus    | http://<SERVER_ADDRESS>:9090                           | Collects and stores metrics in a time-series DB (TSDB) |
| grafana       | http://<SERVER_ADDRESS>:3000 (login/pass: admin/admin) | Visualizes metrics, manages dashboards and alerts      |
| node-exporter | Shared via host network(Port 9100)                     | Exposes host OS metrics                                |
| cadvisor      | http://<SERVER_ADDRESS>:8080                           | Exposes Docker container metrics                       |

## Deployment

1. Create a local `.env` runtime configurations file (optional)
   ```bash
   cp <stack_dir>/.env.example <stack_dir>/.env
   ```
2. Create shared network to the docker monitoring containers:

   ```bash
   docker network create monitoring
   ```

### Server monitoring setup

```bash
for stack in {cadvisor,prometheus,node-exporter,grafana}; do
  docker compose -f "monitoring/$stack/docker-compose.yml" up -d;
done
```

### Remote hosts agents setup

#### 1. On remote host

- **Configure the environment files first as shown in Step 1 of the Deployment section**

* Host metrics:

  ```bash
  docker compose -f node-exporter/docker-compose.yml up -d
  ```

* Docker container metrics:
  **Create shared network as shown in Step 2 of the Deployment section**
  ```bash
  docker compose -f cadvisor/docker-compose.yml up -d
  ```

#### 2. On server monitoring host

1. Add the remote hosts address to `prometheus/config/prometheus.yml`:

   ```yaml
   scrape_configs:
     - job_name: "node-exporter"
       static_configs:
         - targets: ["host.docker.internal:9100"] # Local host (Saturn)
         - targets: ["<REMOTE_HOST_ADDRESS>:9100"] # Remote Node

     - job_name: "cadvisor"
       static_configs:
         - targets: ["cadvisor:8080"] # Local cAdvisor container
         - targets: ["<REMOTE_HOST_ADDRESS>:8080"] # Remote cAdvisor
   ```

2. Apply Prometheus config

   ```bash
   docker compose -f prometheus/docker-compose.yml restart
   ```

## Preinstalled Dashboards

| Dashboard Path                                     | Upstream Source                                                 |
| :------------------------------------------------- | :-------------------------------------------------------------- |
| `Dashboards / Infrastructure / Node Exporter Full` | https://grafana.com/grafana/dashboards/1860-node-exporter-full/ |
| `Dashboards / Infrastructure / Docker monitoring`  | https://grafana.com/grafana/dashboards/15798-docker-monitoring/ |

## Author

Maxim Shandruk
