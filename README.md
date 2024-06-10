# Ansible Playbook to Add a Static Route on FRR
This Ansible playbook is designed to add a static route to devices running Free Range Routing (FRR). It installs FRR if not already present, configures the static route, and restarts the FRR service to apply the changes.

## Directory Structure

Ensure your directory structure looks like this:

```plaintext
ansible/
├── inventory
│   └── hosts
├── playbooks
│   └── add_static_route.yml
└── roles
    └── frr
        ├── handlers
        │   └── main.yml
        ├── tasks
        │   └── main.yml
        └── templates
            └── static_route.j2
