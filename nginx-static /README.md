
# Nginx static site

This example deploys Nginx with the `nginx_web_server` role. The role installs
Nginx, publishes a static index page, and serves a custom page for HTTP 404
responses.

Run it with:

```bash
ansible-playbook -i hosts.ini deploy-nginx-static.yaml \
	--private-key ~/.ssh/tests_gcp
```

The host must be a Debian or Ubuntu system with SSH access. The role defaults
can be overridden from inventory or `group_vars` when needed.
