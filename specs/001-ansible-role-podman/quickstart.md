# Quickstart Guide

This guide provides a basic example of how to use the `podman` Ansible role in a playbook.

## Prerequisites

- Ansible is installed.
- The `ansible-role-podman` role is available in your roles path.

## Example Playbook

Create a file named `playbook.yml` with the following content:

```yaml
---
- name: Install and configure Podman
  hosts: all
  become: true
  vars:
    registries_search:
      - 'docker.io'
      - 'quay.io'
      - 'registry.fedoraproject.org'
    registries_insecure: []
    registries_block: []
  roles:
    - ansible-role-podman
```

## Running the Playbook

Execute the playbook using the `ansible-playbook` command:

```bash
ansible-playbook -i inventory/hosts playbook.yml
```

This will apply the role to the hosts defined in your inventory, installing Podman and configuring the specified registries.
