# Fail2Ban Role

Intrusion prevention via Fail2Ban for Ubuntu 24.04 LTS.

## What it does

- Installs and configures Fail2Ban
- Enables SSH jail with 3-attempt limit by default
- Uses UFW as the ban action (integrates with ufw role)
- Supports custom jails for additional services
- Optional email notifications on bans

## Key Variables

| Variable | Default | Description |
|----------|---------|-------------|
| `fail2ban_bantime` | `1h` | Duration of ban |
| `fail2ban_findtime` | `10m` | Window to count failures |
| `fail2ban_maxretry` | `5` | Max retries before ban |
| `fail2ban_banaction` | `ufw` | Action backend (ufw integration) |
| `fail2ban_jails` | sshd jail | List of jails to configure |
| `fail2ban_ignoreip` | `127.0.0.1/8 ::1` | IPs to never ban |

See `defaults/main.yml` for the full list.
