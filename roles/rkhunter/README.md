# rkhunter Role

Rootkit detection via rkhunter for Ubuntu 24.04 LTS.

## What it does

- Installs rkhunter (Rootkit Hunter)
- Deploys a hardened rkhunter configuration
- Updates file properties database on deploy
- Schedules daily rootkit scans via cron
- Configures APT hook to update database after package upgrades
- Configures log rotation for scan reports
- Optionally emails warnings on detection

## Key Variables

| Variable | Default | Description |
|----------|---------|-------------|
| `rkhunter_enabled` | `true` | Enable rkhunter |
| `rkhunter_cron_enabled` | `true` | Schedule daily scans |
| `rkhunter_cron_schedule` | `0 3 * * *` | Cron schedule (daily at 03:00) |
| `rkhunter_report_path` | `/var/log/rkhunter.log` | Scan log path |
| `rkhunter_mailto` | `""` | Email recipient for warnings |
| `rkhunter_update_db` | `true` | Update properties database on deploy |
| `rkhunter_apt_hook` | `true` | APT hook for automatic DB updates |
| `rkhunter_disable_tests` | `suspscan hidden_ports hidden_procs ...` | Tests to disable |
| `rkhunter_scriptwhitelist` | egrep, fgrep, which, ldd | Known-good script whitelist |

See `defaults/main.yml` for the full list.
