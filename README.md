# Vault Agent Ansible Demo

This demo shows how to deploy and configure HashiCorp Vault Agent using Ansible to:

- Install Vault and Vault Agent as a systemd service
- Configure Vault Agent to:
  - Pull a secret from Vault and write it into `/etc/httpd/djoo.cfg`
  - Inject another secret as an environment variable (`MY_SECRET_VAR`)
  - Render an HTML page at `/var/www/html/index.html` using both values
  - Re-check Vault for changes every 5 seconds and restart `httpd` when changes are detected
- Set up and expose `httpd` on ports 80/443 with `firewalld`

## Vault Requirements

Ensure the following secrets exist:

```bash
# Secret that will go into a config file
vault kv put -namespace=admin vault-agent-secret/file_variable variable="some_config_value"

# Secret that will become an environment variable
vault kv put -namespace=admin vault-agent-secret/env_variable env_var="some_env_value"
```

## Ansible Requirements
- ansible-core >= 2.13
- Host should support dnf, systemd, and firewalld
- ansible.posix collection (for firewalld module):