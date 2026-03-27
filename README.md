# ansible-ops

Infrastructure automation and configuration management with Ansible, aligned with EU and Austrian regulatory requirements.

## Regulatory Scope

This repository enforces technical controls required by:

| Framework | Scope |
|-----------|-------|
| NIS2 Directive (EU 2022/2555) | Art 20–23: risk management, incident handling, reporting |
| NISG 2026 (Austrian transposition) | National NIS2 implementation, effective 2026-10-01 |
| Cyber Resilience Act (EU 2024/2847) | Annex I: secure-by-design, minimal attack surface |
| GDPR (EU 2016/679) / Austrian DSG | Art 5, 25, 32: data protection by design, security of processing |
| ISO/IEC 27001:2022 | Annex A controls (A.5–A.8) |

Controls are cross-referenced to the [platform-assurance](https://github.com/rmednitzer/platform-assurance) control catalog (`CTL-0001` through `CTL-0010`) and ISMS policy set (`POL-01` through `POL-10`).

## Getting Started

### Prerequisites

- [Ansible](https://docs.ansible.com/ansible/latest/installation_guide/) >= 2.15
- Python >= 3.10

### Setup

```bash
# Install Galaxy dependencies
ansible-galaxy install -r requirements.yml
```

### Usage

```bash
# Dry-run a playbook
ansible-playbook -i inventories/<env>/hosts playbooks/site-common.yml --check --diff

# Run a playbook
ansible-playbook -i inventories/<env>/hosts playbooks/site-common.yml

# Run only compliance-tagged roles
ansible-playbook -i inventories/<env>/hosts playbooks/site-common.yml --tags compliance

# Run specific roles
ansible-playbook -i inventories/<env>/hosts playbooks/site-common.yml --tags ssh,firewall,audit
```

## Roles

| Role | Purpose | Key Compliance References |
|------|---------|---------------------------|
| `common` | Base packages, timezone, sysctl, kernel/FS hardening, log retention | NIS2 Art 21.2(a)(e), GDPR Art 25/32, CRA Annex I |
| `users` | User accounts, sudo, password policy, account lockout | NIS2 Art 21.2(i), GDPR Art 32, POL-03 |
| `ntp` | Chrony time synchronisation (Austrian/EU NTP pools, NTS) | CTL-0006, ISO 27001 A.8.17 |
| `ssh_hardening` | SSH server hardening, legal banner, approved ciphers | POL-03, POL-06, NIS2 Art 21.2(h)(i) |
| `ufw` | UFW firewall with default-deny, IPv6, rate limiting | NIS2 Art 21.2(e), GDPR Art 32 |
| `fail2ban` | Intrusion prevention with recidive jail | NIS2 Art 21.2(b), POL-04 |
| `aide` | File integrity monitoring (AIDE) | NIS2 Art 21.2(a), GDPR Art 32, CTL-0004 |
| `rkhunter` | Rootkit detection (hidden processes, kernel modules, signatures) | NIS2 Art 21.2(a)(b), GDPR Art 32, ISO 27001 A.8.7 |
| `log_forwarding` | Centralised log forwarding via rsyslog (TLS) | CTL-0006, NIS2 Art 21.2(a), GDPR Art 5(2) |
| `auditd` | System audit logging (CIS + NIS2/GDPR rules) | CTL-0006, GDPR Art 5(2), NIS2 Art 21.2(a) |

## Repository Structure

```
inventories/          Per-environment inventory (production, staging, development)
playbooks/            Top-level playbooks
roles/                Custom Ansible roles
  common/             Base system management and hardening
  users/              User management and access control
  ntp/                Time synchronisation (chrony)
  ssh_hardening/      SSH server hardening
  ufw/                UFW firewall
  fail2ban/           Intrusion prevention
  aide/               File integrity monitoring
  rkhunter/           Rootkit detection
  log_forwarding/     Centralised log forwarding
  auditd/             System audit logging
group_vars/           Global group variables
host_vars/            Global host-specific variables
plugins/              Custom filter, module, and lookup plugins
files/                Static files used by playbooks
templates/            Jinja2 templates used by playbooks
```

## Evidence and Audit

Each role produces configuration artifacts that serve as compliance evidence:

- **Audit logs** (`/var/log/audit/`) — CIS + NIS2/GDPR rules, immutable
- **AIDE reports** (`/var/log/aide/`) — daily file integrity checks
- **rkhunter reports** (`/var/log/rkhunter.log`) — daily rootkit scans
- **Auth logs** — retained per `common_log_retention.security_audit` (365 days default)
- **SSH banner** — legal monitoring notice per GDPR Art 5(2)
- **Sysctl hardening** — kernel security parameters per CRA Annex I

Log retention tiers are aligned with platform-assurance evidence schema:
- `governance`: 10 years (3650 days)
- `incidents`: 5 years (1825 days)
- `ci`: 3 years (1095 days)
- `dr_tests`: 5 years (1825 days)
- `security_audit`: 1 year (365 days)
- `operational`: 90 days

## License

GNU General Public License v3 - see [LICENSE](LICENSE).
