# NTP Role

NTP time synchronization via chrony for Ubuntu 24.04 LTS.

## What it does

- Removes systemd-timesyncd to avoid conflicts
- Installs and configures chrony
- Configures NTP servers/pools
- Optionally serves time to local network clients
- Verifies synchronization status

## Key Variables

| Variable | Default | Description |
|----------|---------|-------------|
| `ntp_servers` | Ubuntu pool servers | List of NTP servers |
| `ntp_pools` | `[]` | List of NTP pools |
| `ntp_allow_networks` | `[]` | Networks allowed to query as NTP clients |
| `ntp_rtcsync` | `true` | Sync hardware RTC |
| `ntp_makestep_threshold` | `1.0` | Step threshold in seconds |

See `defaults/main.yml` for the full list.
