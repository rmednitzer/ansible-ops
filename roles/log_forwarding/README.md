# Log Forwarding Role

Centralised log forwarding via rsyslog for Ubuntu 24.04 LTS.

## What it does

- Installs and configures rsyslog for remote log forwarding
- Supports TCP, UDP, and RELP forwarding protocols
- Optional TLS encryption for log transport (gnutls)
- Forwards audit logs via audisp-remote
- Configures queue settings for reliability during outages
- Applies rate limiting for data minimisation
- Gracefully skips when no remote server is configured

## Key Variables

| Variable | Default | Description |
|----------|---------|-------------|
| `log_forwarding_enabled` | `true` | Enable log forwarding |
| `log_forwarding_protocol` | `tcp` | Protocol: tcp, udp, or relp |
| `log_forwarding_server` | `""` | Remote syslog server address |
| `log_forwarding_port` | `514` | Remote syslog port |
| `log_forwarding_tls_enabled` | `false` | Enable TLS encryption |
| `log_forwarding_tls_ca_cert` | `""` | TLS CA certificate path |
| `log_forwarding_audit` | `true` | Forward auditd logs via audisp |
| `log_forwarding_queue_size` | `10000` | Queue size for reliability |
| `log_forwarding_queue_type` | `LinkedList` | Queue type |
| `log_forwarding_rate_limit_interval` | `5` | Rate limit interval (seconds) |
| `log_forwarding_rate_limit_burst` | `200` | Rate limit burst |

See `defaults/main.yml` for the full list.
