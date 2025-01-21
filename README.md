# Monitoring Stack Deployment

This Ansible project deploys a monitoring stack consisting of:
- Prometheus
- Grafana
- Alertmanager

## Prerequisites

- Ansible 2.9+
- Target Ubuntu 24.04 ARM server
- SSH access to the target server

## Configuration

1. Update the inventory file with your server details
2. Configure variables in group_vars/all.yml
3. Set up your alert manager configuration (Telegram token and chat ID)

## Usage

```bash
# Test connection
ansible all -m ping

# Deploy the stack
ansible-playbook site.yml
```

```
./
├── README.md
├── ansible.cfg
├── documentation-en.md
├── documentation-ru.md
├── group_vars
│   ├── all.yml
├── inventory
│   ├── hosts.yml
├── requirements.yml
├── roles
│   ├── bootstrap
│   │   ├── handlers
│   │   │   └── main.yml
│   │   └── tasks
│   │       └── main.yml
│   ├── docker
│   │   ├── defaults
│   │   │   └── main.yml
│   │   ├── handlers
│   │   │   └── main.yml
│   │   └── tasks
│   │       └── main.yml
│   └── monitoring
│       ├── defaults
│       │   └── main.yml
│       ├── files
│       │   ├── blackbox-dashboard.json
│       │   ├── mysql-dashboard.json
│       │   └── node-exporter-dashboard.json
│       ├── handlers
│       │   └── main.yml
│       ├── tasks
│       │   └── main.yml
│       └── templates
│           ├── alertmanager.yml.j2
│           ├── alerts.yml.j2
│           ├── docker-compose.yml.j2
│           ├── grafana
│           │   └── provisioning
│           │       ├── dashboards
│           │       │   └── dashboards.yml.j2
│           │       └── datasources
│           │           └── datasources.yml.j2
│           └── prometheus.yml.j2
└── site.yml
```
