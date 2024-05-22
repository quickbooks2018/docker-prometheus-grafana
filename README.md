# Docker Prometheus Grafana


- prometheus.yml
```yml
global:
  scrape_interval: 15s

scrape_configs:
  - job_name: 'prometheus'
    static_configs:
      - targets: ['localhost:9090']

  - job_name: 'node_exporter'
    static_configs:
      - targets: ['node_exporter:9100']
```

- docker-compose.yml
```yml
---
services:
  prometheus:
    image: prom/prometheus:latest
    container_name: prometheus
    volumes:
      - .${PWD}/prometheus.yml:/etc/prometheus/prometheus.yml
    command:
      - '--config.file=/etc/prometheus/prometheus.yml'
      - '--storage.tsdb.path=/prometheus'
      - '--web.console.libraries=/usr/share/prometheus/console_libraries'
      - '--web.console.templates=/usr/share/prometheus/consoles'
    ports:
      - '9090:9090'
    restart: unless-stopped
    networks:
      - monitoring

  grafana:
    image: grafana/grafana:latest
    container_name: grafana
    volumes:
      - grafana-storage:/var/lib/grafana
    environment:
      - GF_SECURITY_ADMIN_USER=admin
      - GF_SECURITY_ADMIN_PASSWORD=admin
    ports:
      - '3000:3000'
    restart: unless-stopped
    networks:
      - monitoring

  node_exporter:
    image: prom/node-exporter:latest
    container_name: node_exporter
    ports:
      - '9100:9100'
    restart: unless-stopped
    networks:
      - monitoring

networks:
  monitoring:
    driver: bridge

volumes:
  grafana-storage:
```

- docker compose command
```bash
docker compose -p prometheus up -d
```

- Play with Docker
```bash
ssh-keygen -t ed25519 -f cloudgeeks -C "default"
ssh -i ~/.ssh/cloudgeeks  root@ip172-18-0-31-cnn5hu291nsg00dv9vk0@direct.labs.play-with-docker.com
```

- Prometheus Grafana Dashboards

- https://grafana.com/grafana/dashboards/3662-prometheus-2-0-overview/

- https://grafana.com/grafana/dashboards/3681-prometheus2-0/