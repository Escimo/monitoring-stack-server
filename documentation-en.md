# Monitoring System Based on Prometheus and Grafana

## Table of Contents
1. System Overview
2. Requirements
3. Installation
4. Configuration
5. Core Components
6. Maintenance
7. Troubleshooting

## 1. System Overview
This monitoring system is built using the following components:
- Prometheus: Collects and stores metrics.
- Grafana: Visualizes data.
- Alertmanager: Manages alerts.

Automatic tracking setup for exporters:
- Node Exporter: System metrics.
- cAdvisor: Container metrics.
- MySQL Exporter: MySQL metrics.
- Blackbox Exporter: HTTP endpoint monitoring.

All dependencies are installed automatically.
Components are deployed in Docker containers using Docker-Compose.

## 2. Requirements
- Ubuntu 24.04 ARM
- Ansible for deployment

## 3. Installation

### Environment Preparation
```bash
# Clone the repository
git clone https://github.com/fossasoftware/containerized-monitoring-server.git
cd containerized-monitoring-server

# Set up inventory
cp inventory/hosts.yml.example inventory/hosts.yml
# Edit inventory/hosts.yml to specify your server
```

### Deployment
```bash
# Check host availability
ansible all -m ping

# Run the playbook
ansible-playbook site.yml
```

## 4. Configuration

### Prometheus
- Configuration path: `/opt/monitoring/prometheus/prometheus.yml`
- Data retention period: 30 days
- Scrape interval: 15 seconds

### Alertmanager
- Configuration path: `/opt/monitoring/alertmanager/alertmanager.yml`
- Configured for Telegram notifications
- Alert grouping by: alertname, job, severity

### Grafana
- Access URL: http://your-server:3000
- Default login: admin
- Default password: admin
- Pre-installed dashboards:
  - Node Exporter Dashboard
  - MySQL Overview
  - Blackbox Monitoring

## 5. Core Components

### Node Exporter
- Port: 9100
- Metrics: CPU, memory, disk, network
- Alerts:
  - HighCPUUsage (>85%)
  - CriticalCPUUsage (>95%)
  - HighMemoryUsage (>85%)
  - HighDiskUsage (>85%)

### MySQL Exporter
- Port: 9104
- Key metrics:
  - Connection count
  - Query execution rate
  - Replication status
  - Database size

### Blackbox Exporter
- HTTP endpoints availability monitoring
- SSL certificate monitoring
- Service response time

## 6. Maintenance

### Component Updates
```bash
cd /opt/monitoring
docker-compose pull
docker-compose up -d
```

### Status Check
```bash
docker-compose ps
docker-compose logs -f [service-name]
```

### Backup
Backup is not configured in the current setup.

## 7. Troubleshooting

### Log Inspection
```bash
# Prometheus logs
docker-compose logs prometheus

# Alertmanager logs
docker-compose logs alertmanager

# Grafana logs
docker-compose logs grafana
```

### Common Issues

1. Prometheus not collecting metrics:
   - Check exporters availability
   - Verify prometheus.yml configuration
   - Check firewall settings

2. Notifications not working:
   - Check Alertmanager configuration
   - Verify Telegram token and chat_id
   - Check Alertmanager logs

3. Grafana not displaying data:
   - Check Prometheus connection
   - Verify dashboard queries
   - Check access permissions

### Useful Commands
```bash
# Restart all services
docker-compose restart

# Check Prometheus configuration
docker-compose exec prometheus promtool check config /etc/prometheus/prometheus.yml

# Check alert rules
docker-compose exec prometheus promtool check rules /etc/prometheus/alerts.yml
```
