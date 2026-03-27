# CLAUDE.md - AI Assistant Guide for ansible-ops

## Project Overview

This is an Ansible operations repository for infrastructure automation and configuration management, aligned with EU and Austrian regulatory requirements. Licensed under GNU GPLv3.

## Compliance Alignment

This repository implements technical controls mapped to:

- **NIS2 Directive (EU 2022/2555)** — Art 20-23: risk management, incident handling, reporting
- **NISG 2026 (Austrian transposition)** — effective 2026-10-01
- **Cyber Resilience Act (EU 2024/2847)** — Annex I: secure-by-design
- **GDPR (EU 2016/679) / Austrian DSG** — Art 5, 25, 32: data protection by design
- **ISO/IEC 27001:2022** — Annex A controls

Controls and policies are defined locally in `docs/compliance-controls.yml`:
- Control catalog: `CTL-0001`, `CTL-0002`, `CTL-0003`
- ISMS policies: `POL-01`, `POL-02`, `POL-03`, `POL-04`, `POL-05`

When modifying roles, preserve compliance cross-references in `defaults/main.yml` headers.
When adding new controls, map them to the relevant regulatory articles and add definitions to `docs/compliance-controls.yml`.

## Repository Structure

This repository follows the standard Ansible best-practices layout:

```
ansible-ops/
├── CLAUDE.md              # This file - AI assistant guide
├── LICENSE                # GNU General Public License v3
├── README.md              # Project documentation
├── .gitignore             # Git ignore rules
├── ansible.cfg            # Ansible configuration
├── requirements.yml       # Ansible Galaxy dependencies (roles/collections)
├── inventories/           # Inventory files organized by environment
│   ├── production/
│   │   ├── hosts          # Production host definitions
│   │   ├── group_vars/    # Production group variables
│   │   └── host_vars/     # Production host-specific variables
│   ├── staging/
│   │   ├── hosts
│   │   ├── group_vars/
│   │   └── host_vars/
│   └── development/
│       ├── hosts
│       ├── group_vars/
│       └── host_vars/
├── playbooks/             # Top-level playbooks
├── roles/                 # Custom Ansible roles
│   └── <role_name>/
│       ├── tasks/
│       ├── handlers/
│       ├── templates/
│       ├── files/
│       ├── vars/
│       ├── defaults/
│       ├── meta/
│       └── README.md
├── group_vars/            # Global group variables
│   └── all.yml
├── host_vars/             # Global host-specific variables
├── plugins/               # Custom plugins
│   ├── filter/
│   ├── modules/
│   └── lookup/
├── files/                 # Static files used by playbooks
└── templates/             # Jinja2 templates used by playbooks
```

## Current State

The repository contains a complete compliance-aligned hardening baseline with 9 roles:
`common`, `users`, `ntp`, `ssh_hardening`, `ufw`, `fail2ban`, `aide`, `log_forwarding`, `auditd`.

The main playbook `playbooks/site-common.yml` applies all roles to Ubuntu 24.04 LTS hosts.

## Development Conventions

### Naming Conventions

- **Roles**: Use lowercase with underscores (e.g., `nginx_proxy`, `postgresql_server`)
- **Playbooks**: Use lowercase with hyphens (e.g., `deploy-app.yml`, `setup-monitoring.yml`)
- **Variables**: Use lowercase with underscores, prefixed by role name (e.g., `nginx_listen_port`, `postgres_max_connections`)
- **Files/Templates**: Use lowercase with hyphens or underscores, matching the target filename
- **Inventory groups**: Use lowercase with underscores (e.g., `web_servers`, `db_servers`)
- **Tags**: Use lowercase with hyphens (e.g., `install-packages`, `configure-service`)

### YAML Style

- Use `.yml` extension (not `.yaml`)
- Use 2-space indentation
- Always use `true`/`false` for booleans (not `yes`/`no`)
- Quote strings that contain special YAML characters or could be misinterpreted
- Start every YAML file with `---`
- Use block style (`key: value`) over flow style (`{key: value}`)

### Ansible Best Practices

- Always name tasks descriptively (every `- name:` should explain what the task does)
- Use fully qualified collection names (FQCNs) for modules (e.g., `ansible.builtin.copy` not just `copy`)
- Prefer `ansible.builtin.template` over `ansible.builtin.copy` for config files that need variable substitution
- Use `become: true` only when needed, not globally
- Keep secrets in Ansible Vault encrypted files; never commit plaintext secrets
- Use `ansible.builtin.import_tasks` for static includes and `ansible.builtin.include_tasks` for dynamic includes
- Use handlers for service restarts/reloads triggered by configuration changes
- Set `changed_when` and `failed_when` on shell/command tasks to ensure accurate reporting

### Role Structure

Every role should include:
- `defaults/main.yml` - Default variables (overridable)
- `tasks/main.yml` - Main task list
- `handlers/main.yml` - Handlers (if needed)
- `meta/main.yml` - Role metadata and dependencies
- `README.md` - Role documentation

### Variable Precedence

Follow Ansible's variable precedence. Prefer defining variables at these levels:
1. `roles/<role>/defaults/main.yml` - Role defaults (lowest precedence, most overridable)
2. `group_vars/` - Group-specific overrides
3. `host_vars/` - Host-specific overrides
4. Playbook `vars:` - Playbook-level overrides (use sparingly)

### Secrets Management

- Use `ansible-vault` for encrypting sensitive data
- Store vault-encrypted files with a `vault_` prefix (e.g., `vault_secrets.yml`)
- Never commit unencrypted secrets, passwords, API keys, or private keys
- Reference vault variables with a `vault_` prefix in variable names

## Common Commands

```bash
# Check syntax of a playbook
ansible-playbook playbooks/<playbook>.yml --syntax-check

# Dry-run a playbook (check mode)
ansible-playbook -i inventories/<env>/hosts playbooks/<playbook>.yml --check --diff

# Run a playbook
ansible-playbook -i inventories/<env>/hosts playbooks/<playbook>.yml

# Run with specific tags
ansible-playbook -i inventories/<env>/hosts playbooks/<playbook>.yml --tags "tag1,tag2"

# Lint all YAML/Ansible files
ansible-lint
yamllint .

# Encrypt a file with vault
ansible-vault encrypt <file>

# View an encrypted file
ansible-vault view <file>

# Install Galaxy dependencies
ansible-galaxy install -r requirements.yml
```

## Quality Tools

When adding CI or local tooling, use:
- **ansible-lint** - Ansible-specific linting
- **yamllint** - YAML syntax and style checking
- **molecule** - Role testing framework (if testing roles in isolation)

## Git Workflow

- Branch from `main` for feature work
- Use descriptive branch names (e.g., `feature/add-nginx-role`, `fix/postgres-permissions`)
- Write clear commit messages describing the "why" of changes
- Keep commits focused on a single logical change

## Important Notes for AI Assistants

- Always read existing files before modifying them
- Never commit plaintext secrets or credentials
- When creating new roles, follow the full role structure outlined above
- When modifying inventory, be careful about environment separation
- Prefer idempotent operations in all Ansible tasks
- Test playbooks with `--check --diff` before actual runs when possible
- Use FQCNs for all module references
