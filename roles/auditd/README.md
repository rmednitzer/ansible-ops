# Auditd Role

System audit logging via auditd for Ubuntu 24.04 LTS.

## What it does

- Installs auditd and audispd-plugins
- Configures log rotation and disk space handling
- Deploys CIS Benchmark-aligned audit rules covering:
  - Time changes, user/group modifications, network changes
  - Login/logout tracking, permission modifications
  - Unauthorized access attempts, privileged command usage
  - Kernel module loading, file deletion, sudoers changes
- Makes audit rules immutable (requires reboot to modify)
- Supports custom rules

## Key Variables

| Variable | Default | Description |
|----------|---------|-------------|
| `auditd_buffer_size` | `8192` | Audit event buffer size |
| `auditd_max_log_file` | `50` | Max log file size (MB) |
| `auditd_num_logs` | `10` | Number of rotated logs |
| `auditd_cis_rules` | `true` | Enable CIS Benchmark rules |
| `auditd_custom_rules` | `[]` | Additional raw audit rules |
| `auditd_failure_mode` | `1` | 0=silent, 1=printk, 2=panic |

See `defaults/main.yml` for the full list.
