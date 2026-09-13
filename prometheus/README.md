# Prometheus server

This example deploys Prometheus on a Debian or Ubuntu server with the
`prometheus_server` role. It creates a dedicated system user with no login
shell, installs Prometheus from the official release archive, and runs it as a
systemd service.

UFW is enabled with incoming traffic denied by default. SSH and Prometheus on
port 9090 are allowed. Restrict the Prometheus source networks before using
this outside a private network by overriding
`prometheus_firewall_allowed_sources` in inventory or `group_vars`.

Install the required Ansible collection and run the playbook:

```bash
ansible-galaxy collection install -r requirements.yml
ansible-playbook -i hosts.ini deploy-prometheus.yml \
  --private-key ~/.ssh/tests_gcp
```

Set `prometheus_checksum` to the SHA-256 checksum published for the selected
Prometheus release when checksum verification is required.