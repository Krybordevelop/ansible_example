# WireGuard VPN Server

This project automates deployment of a WireGuard-based VPN server with the WGDashboard web interface.

## What is installed

The playbook performs the following tasks:

- installs WireGuard, WireGuard Tools, and required dependencies;
- enables IPv4 forwarding;
- clones the WGDashboard project from GitHub;
- creates a Python virtual environment and installs dependencies using `./wgd.sh install`;
- creates the `wgdashboard` systemd service;
- starts the service and verifies its status.

By default, WGDashboard is available on port `10086`.

## Project structure

- `deploy_wireguard_srv.yml` — main Ansible playbook;
- `hosts.ini` — inventory file for the target server;
- `README.MD` — project description.
- `templates/wgdashboard.service.j2` — templete for  Systemd service

## Requirements

- a managed host based on Debian/Ubuntu or another compatible Linux distribution;
- Ansible installed;
- SSH access to the server using a user with `sudo` privileges;
- internet access to download dependencies and the WGDashboard repository.

## Inventory configuration

Edit the `hosts.ini` file and specify the address or hostname of the target server:

```ini
[vpn_servers]
vpn.local
```

If needed, you can add connection parameters:

```ini
[vpn_servers]
vpn.local ansible_user=deploy ansible_ssh_private_key_file=~/.ssh/id_rsa
```

## Running the installation

From the project directory, run:

```bash
ansible-playbook -i hosts.ini deploy_wireguard_srv.yml
```

## Verification after installation

Once the service is started successfully, WGDashboard will be available at:

```text
http://<SERVER_IP>:10086
```

The service is managed through `systemd` and can be checked with:

```bash
sudo systemctl status wgdashboard
sudo systemctl restart wgdashboard
```

## Notes

- The WGDashboard web interface is used for the initial configuration and management of WireGuard clients.
- After the first login, you will need to configure VPN settings and clients through WGDashboard itself.
- The `hosts.ini` file should contain the real target host, not just the example `vpn.local`.

## Useful links

- https://www.wireguard.com/
- https://github.com/donaldzou/WGDashboard
