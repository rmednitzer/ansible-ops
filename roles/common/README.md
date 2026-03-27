# Common Role

Base system management for Ubuntu 24.04 LTS (Noble Numbat).

## What it does

- Installs essential system packages
- Configures timezone and locale
- Applies sysctl kernel tuning (network performance + security)
- Sets up unattended-upgrades for automatic security updates
- Configures systemd journal size limits
- Deploys a system info MOTD banner

## Key Variables

| Variable | Default | Description |
|----------|---------|-------------|
| `common_timezone` | `UTC` | System timezone |
| `common_locale` | `en_US.UTF-8` | System locale |
| `common_packages` | *(see defaults)* | List of packages to install |
| `common_auto_updates_enabled` | `true` | Enable unattended-upgrades |
| `common_auto_updates_reboot` | `false` | Auto-reboot after kernel updates |
| `common_sysctl_settings` | *(see defaults)* | Dict of sysctl key/value pairs |
| `common_journal_max_size` | `500M` | Max systemd journal disk usage |

See `defaults/main.yml` for the full list.
