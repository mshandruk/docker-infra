# monitoring

A monitoring stack powered Prometheus, Grafana, cAdvisor and Node Exporter.

## Structure

```text
├── docker-compose.yml
├── README.md
├── prometheus/
│   └── prometheus.yml
└── grafana/
    └── provisioning/
        ├── dashboards/
        │   ├── dashboards.yaml
        │   └── my-dashboards/       # User dashboards in JSON format are stored here.
        └── datasources/
            └── datasources.yaml     # Automatically binds dashboards to Prometheus.

```

## Services

| Service       | Access                                                 | Description                                             |
| :------------ | :----------------------------------------------------- | :------------------------------------------------------ |
| prometheus    | http://<SERVER_ADDRESS>:9090                           | Collects and stores metrcics in a time-series DB (TSDB) |
| grafana       | http://<SERVER_ADDRESS>:3000 (login/pass: admin/admin) | Visualizes metrcis, manages dashboards and alerts       |
| node-exporter |                                                        | Collects host OS metrics                                |
| cadvisor      | http://<SERVER_ADDRESS>:8080                           | Collects Docker container metrics                       |

## Deploy

1. Create a `.env` configurations file (optional)

```bash
cp .env.example .env
```

2. Deploy the stack

```bash
docker compose up -d
```

## Preinstalled Dashboards

| Dashboard Path                                     | Upstream Source                                                 |
| :------------------------------------------------- | :-------------------------------------------------------------- |
| `Dashboards / Infrastructure / Node Exporter Full` | https://grafana.com/grafana/dashboards/1860-node-exporter-full/ |
| `Dashboards / Infrastructure / Docker monitoring`  | https://grafana.com/grafana/dashboards/15798-docker-monitoring/ |

## Author

Maxim Shandruk
