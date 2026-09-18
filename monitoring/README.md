# monitoring

A monitoring stack powered Prometheus + Grafana + Node Exporter.

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

## Deploy

1. Create a `.env` configurations file (optional)

```bash
cp .env.example .env
```

2. Deploy the stack

```bash
docker compose up -d
```

Pre-installed dashboards are available on the `Dashboards` tab.

## Author

Maxim Shandruk
