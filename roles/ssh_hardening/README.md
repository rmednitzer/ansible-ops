# SSH Hardening Role

SSH server hardening for Ubuntu 24.04 LTS.

## What it does

- Deploys a hardened sshd_config with modern ciphers and algorithms
- Disables password authentication by default (key-only)
- Restricts root login to key-based only
- Removes weak Diffie-Hellman moduli
- Configures session timeouts and max auth attempts
- Deploys a warning banner
- Optionally restricts access to specific users/groups

## Key Variables

| Variable | Default | Description |
|----------|---------|-------------|
| `ssh_port` | `22` | SSH listen port |
| `ssh_permit_root_login` | `prohibit-password` | Root login policy |
| `ssh_password_authentication` | `false` | Allow password auth |
| `ssh_max_auth_tries` | `3` | Max authentication attempts |
| `ssh_client_alive_interval` | `300` | Idle timeout (seconds) |
| `ssh_allowed_users` | `[]` | Restrict to specific users |
| `ssh_allowed_groups` | `[]` | Restrict to specific groups |

See `defaults/main.yml` for the full list.
