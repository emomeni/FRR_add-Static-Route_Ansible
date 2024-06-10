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
```

## Inventory File (`inventory/hosts`)

Define your target hosts in an inventory file.
```ini
[frr_routers]
router1 ansible_host=192.168.1.1 ansible_user=your_user ansible_password=your_password
```

## Playbook (playbooks/add_static_route.yml)
Create a playbook to execute the role.

```ini
- name: Add static route to FRR routers
  hosts: frr_routers
  become: yes
  roles:
    - frr
  vars:
    static_route:
      destination: "10.0.0.0/24"
      next_hop: "192.168.1.254"
```

## Role Tasks (roles/frr/tasks/main.yml)
Define the tasks to configure the static route.
```ini
- name: Ensure FRR is installed
  package:
    name: frr
    state: present

- name: Template the static route configuration
  template:
    src: static_route.j2
    dest: /etc/frr/staticd.conf
    owner: frr
    group: frr
    mode: 0644
  notify: restart frr

- name: Ensure FRR service is running
  service:
    name: frr
    state: started
    enabled: yes
```

## Template (roles/frr/templates/static_route.j2)
Create a template file to define the static route configuration.
```ini
ip route {{ static_route.destination }} {{ static_route.next_hop }}
```

## Handlers (roles/frr/handlers/main.yml)
Define handlers to restart FRR if the configuration changes.
```ini
- name: restart frr
  service:
    name: frr
    state: restarted
```

## Variables
You can define variables in a vars directory or directly in the playbook. For simplicity, let's include it in the playbook as shown above.

## Running the Playbook
Navigate to your ansible directory and run the playbook with the following command:
```ini
ansible-playbook -i inventory/hosts playbooks/add_static_route.yml
```

## Customizing the Static Route
To customize the static route, modify the destination and next_hop variables in the playbook:
```ini
vars:
  static_route:
    destination: "your_destination_network"
    next_hop: "your_next_hop_ip"
```

## Execute the Playbook
Navigate to your ansible directory and run the playbook with the following command:
```ini
ansible-playbook -i inventory/hosts playbooks/add_static_route.yml

This playbook ensures FRR is installed, templates the static route configuration into the staticd.conf file, and restarts the FRR service to apply the changes.
You can modify the destination and next_hop variables to suit your requirements.
```

## Requirements
```plaintext
Ansible 2.9 or later
Devices running Free Range Routing (FRR)
SSH access to the target devices with appropriate user permissions
```

## Author
This playbook was created by Ehsan Momeni Bashusqeh.
