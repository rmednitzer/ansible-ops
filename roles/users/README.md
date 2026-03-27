# Users Role

User management, sudo hardening, and password policy for Ubuntu 24.04 LTS.

## What it does

- Configures password aging and complexity via PAM/pwquality
- Hardens sudo with logging, TTY requirement, and PATH restriction
- Creates managed user accounts with SSH key deployment
- Sets restrictive home directory permissions (0750)
- Configures system-wide umask and login timeout
- Disables shells on unused system accounts
- Optionally locks root account

## Key Variables

| Variable | Default | Description |
|----------|---------|-------------|
| `users_managed` | `[]` | List of user accounts to manage |
| `users_sudo_passwordless` | `false` | Allow passwordless sudo |
| `users_sudo_require_tty` | `true` | Require TTY for sudo |
| `users_password_max_days` | `90` | Password expiry (days) |
| `users_password_min_length` | `12` | Minimum password length |
| `users_umask` | `027` | Default umask |
| `users_lock_root` | `false` | Lock root account |
| `users_disable_system_accounts` | `true` | Disable unused system shells |

See `defaults/main.yml` for the full list.
