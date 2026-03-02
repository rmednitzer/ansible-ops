# ansible-ops

Infrastructure automation and configuration management with Ansible.

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
ansible-playbook -i inventories/<env>/hosts playbooks/<playbook>.yml --check --diff

# Run a playbook
ansible-playbook -i inventories/<env>/hosts playbooks/<playbook>.yml
```

## Repository Structure

```
inventories/          Per-environment inventory (production, staging, development)
playbooks/            Top-level playbooks
roles/                Custom Ansible roles
group_vars/           Global group variables
host_vars/            Global host-specific variables
plugins/              Custom filter, module, and lookup plugins
files/                Static files used by playbooks
templates/            Jinja2 templates used by playbooks
```

## License

GNU General Public License v3 - see [LICENSE](LICENSE).
