<!--
SYNC IMPACT REPORT

- Version: 0.0.0 -> 1.0.0
- Change Type: MAJOR (Initial Ratification)
- Summary: The project constitution has been created and ratified from the user's detailed input, establishing formal principles for the Ansible role's development.

- Sections Added:
  - 1. Code Quality and Style
  - 2. Testing
  - 3. Architecture and Patterns
  - 4. Documentation
  - Governance

- Templates Requiring Review:
  - [ ] .specify/templates/plan-template.md
  - [ ] .specify/templates/spec-template.md
  - [ ] .specify/templates/tasks-template.md
  - [ ] .specify/templates/commands/
  - [ ] README.md

- Follow-up TODOs:
  - None
-->

# Project Constitution: Ansible Role

This constitution defines the principles and rules for the development of this Ansible role. Development will be guided at all times by the standards and best practices of the Ansible and Molecule communities to ensure a robust, maintainable, and well-tested role. The AI agent must follow these rules at all times.

## 1. Code Quality and Style

- **Linting:** All YAML and Ansible code must be validated with `ansible-lint` and `yamllint` using the repository's configurations. No changes should be merged if linting fails.
- **Formatting:** YAML files must have an indentation of 2 spaces.
- **Naming:**
  - Variables must be in lowercase and separated by underscores (e.g., `podman_version`).
  - Task names must be descriptive and start with an infinitive verb (e.g., "Install podman package").

## 2. Testing

- **Framework and Methodology:** `Molecule` will be used with the `ansible` driver and a **native Ansible inventory**. This approach gives us full control over the testing infrastructure using standard Ansible playbooks.

- **Compatibility and Inventory Structure:** The role must be tested on the Debian, RHEL, and Arch Linux distribution families. The test instances will be defined in an Ansible inventory (`inventory/hosts.yml`) as follows:

  *In `molecule.yml`*:
  ```yaml
  driver:
    name: ansible
  ```

  *In `inventory/hosts.yml`*:
  ```yaml
  ---
  molecule:
    hosts:
      debian-instance:
        image: "docker.io/debian:latest"
        command: "/sbin/init"
        privileged: true
      rocky-instance:
        image: "docker.io/rockylinux:latest"
        command: "/sbin/init"
        privileged: true
      arch-instance:
        image: "docker.io/archlinux:latest"
        command: "/sbin/init"
        privileged: true
      ubuntu-instance:
        image: "docker.io/ubuntu:latest"
        command: "/sbin/init"
        privileged: true
      fedora-instance:
        image: "docker.io/fedora:latest"
        command: "/sbin/init"
        privileged: true
  ```

- **Container Management:** The `create.yml` and `destroy.yml` playbooks will be responsible for creating and destroying the test containers (using `containers.podman.podman_container`) based on the inventory definitions.

- **Required Scenarios:** The default scenario (`default`) must include at least the following `molecule` sequence: `lint`, `syntax`, `create`, `converge`, `idempotence`, `verify`, `destroy`.

- **Idempotency:** All role executions must be idempotent. The Molecule idempotency test must not report any changes.

## 3. Architecture and Patterns

- **Idempotency (Non-Negotiable):** Every task must be designed to be inherently idempotent. A successful execution of the role, followed by a second immediate execution, **must not** report any changes (`changed=0`).

- **Modularity and Reusability:** The role must be self-contained and reusable. Complex tasks will be divided into smaller, specific files within `tasks/` (e.g., `install.yml`, `configure.yml`, `service.yml`) to facilitate readability and maintenance.

- **Clarity and Maintainability:** The names of tasks, variables, and files must be descriptive and intuitive. Code with complex or non-obvious logic must include comments that explain the "why" of the implementation, not the "what".

- **Security by Default:** It is strictly forbidden to include secrets or sensitive data in plain text. `ansible-vault` must be used for all confidential information.

- **Default Variables:** All user-configurable variables must have a sensible default value in `defaults/main.yml` to facilitate their use and prevent errors.

## 4. Documentation

- **README:** The `README.md` must include an "Input Variables" section that documents all variables from `defaults/main.yml`, explaining their purpose and possible values.
- **Comments:** Complex code or non-obvious logic must have explanatory comments.

## Governance

**Version**: 1.0.0 | **Ratified**: 2025-10-21 | **Last Amended**: 2025-10-21