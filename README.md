# Ansible Examples

This repository contains a set of Ansible playbooks and configuration examples for deploying and managing common infrastructure services on Linux hosts.

The project is intended for learning, practice, and testing automation workflows with Ansible in a real environment.

## Included examples

- Jenkins
  - deployment of Jenkins master and follower nodes;
  - installs required packages such as Docker, Maven, and JDK;
  - includes host inventory and deployment playbooks.

- PostgreSQL
  - configuration for PostgreSQL installation and setup;
  - includes user/database creation tasks and template-based configuration.

- Squid server
  - deployment of a proxy server with custom configuration;
  - useful for caching and traffic filtering experiments.

- WireGuard VPN
  - automated setup of a WireGuard VPN server;
  - includes WGDashboard for web-based management and monitoring.

## Repository structure

```text
ansible_example/
├── Jenkins/
│   ├── deploy-follower.yml
│   ├── deploy-leader.yml
│   ├── host.ini
│   └── README.md
├── postgress/
│   ├── deploy.yml
│   ├── host.ini
│   ├── README.md
│   ├── group_vars/
│   ├── roles/
│   ├── templates/
│   └── vars/
├── squid_server_nochache/
│   ├── deploy_squid.yml
│   ├── host.ini
│   ├── templates/
│   └── vars/
├── vpn_wireguard/
│   ├── deploy_wireguard_srv.yml
│   ├── hosts.ini
│   ├── README.MD
│   └── ...
├── requirements.txt
├── README.md
└── .ansible-lint
```

## Requirements

- Ansible installed on the control machine;
- SSH access to target Linux servers;
- sudo privileges on managed hosts;
- Debian/Ubuntu-based systems for most examples.

## Typical usage

To run an example playbook, go to the corresponding directory and execute:

```bash
ansible-playbook -i hosts.ini deploy.yml
```

or for a specific example:

```bash
ansible-playbook -i host.ini deploy-leader.yml
```

## Purpose

This repository is designed to demonstrate practical Ansible automation patterns, basic service deployment, configuration templating, inventory management, and system administration tasks in a hands-on way.
