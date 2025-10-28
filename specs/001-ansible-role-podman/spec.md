# Feature Specification: Ansible Role for Podman Installation

**Feature Branch**: `001-ansible-role-podman`
**Created**: 2025-10-21
**Status**: Draft
**Input**: User description: "Create an Ansible role named 'podman' that installs and configures Podman in an idempotent manner. **Purpose:** The objective is to standardize the installation of Podman across all our development and production environments to ensure consistency and facilitate management. **Compatibility:** The role must be compatible with the following Linux distribution families: - Debian family (e.g., Debian, Ubuntu) - RHEL family (e.g., Rocky Linux, Fedora, CentOS) - Arch Linux family **Key Features:** - The role must install the 'podman' package using the official repositories of each distribution. - The role must ensure that the 'podman.socket' service (or the equivalent main service) is enabled and running after installation. - The role must allow configuration of search, insecure, and blocked container registries via dedicated Ansible variables."

## Clarifications

### Session 2025-10-21
- Q: What should the variable structure for providing the list of registries look like? → A: The role will use three separate list variables: `registries_search`, `registries_insecure`, and `registries_block`.


## User Scenarios & Testing *(mandatory)*

### User Story 1 - Install Podman on a supported distribution (Priority: P1)

As a system administrator, I want to apply the Ansible role to a server running a supported Linux distribution (Debian, RHEL, or Arch family) so that Podman is installed and running.

**Why this priority**: This is the core functionality of the role. Without it, nothing else matters.

**Independent Test**: This can be tested by running the Ansible playbook with this role on a clean machine of each supported distribution family and verifying that Podman is installed and the service is running.

**Acceptance Scenarios**:

1.  **Given** a server running a supported Debian-family Linux distribution without Podman installed, **When** the Ansible role is applied, **Then** the `podman` package is installed and the `podman.socket` service is enabled and running.
2.  **Given** a server running a supported RHEL-family Linux distribution without Podman installed, **When** the Ansible role is applied, **Then** the `podman` package is installed and the `podman.socket` service is enabled and running.
3.  **Given** a server running a supported Arch-family Linux distribution without Podman installed, **When** the Ansible role is applied, **Then** the `podman` package is installed and the `podman.socket` service is enabled and running.
4.  **Given** a server with Podman already installed, **When** the Ansible role is applied, **Then** the role runs without errors and the Podman installation is unchanged (idempotency).

---

### User Story 2 - Configure Registry Lists (Priority: P2)

    As a system administrator, I want to provide lists for search, insecure, and blocked registries to the Ansible role so that Podman's `registries.conf` is configured accordingly.

    **Why this priority**: This is a key configuration feature that allows customization for different environments.

    **Independent Test**: This can be tested by applying the role with variables for `registries_search`, `registries_insecure`, and `registries_block`, and then inspecting the `/etc/containers/registries.conf` file on the target machine.

    **Acceptance Scenarios**:

    1.  **Given** a server with the Podman role applied, **When** the role is re-applied with a list of registries in the `registries_search` variable, **Then** the `[registries.search]` section in `registries.conf` is updated.
    2.  **Given** a server with the Podman role applied, **When** the role is re-applied with a list of registries in the `registries_insecure` variable, **Then** the `[registries.insecure]` section in `registries.conf` is updated.
    3.  **Given** a server with the Podman role applied, **When** the role is re-applied with a list of registries in the `registries_block` variable, **Then** the `[registries.block]` section in `registries.conf` is updated.

---

### Edge Cases

- What happens when the role is applied to an unsupported distribution? The role should fail with a clear error message.
- What happens if the `podman` package is not available in the distribution's repositories? The role should fail and report the issue.
- What happens if an invalid registry URI is provided? The role must validate the URI format. If the format is invalid, the Ansible playbook execution must fail with a clear error message.

## Requirements *(mandatory)*

### Functional Requirements

-   **FR-001**: The role MUST install the `podman` package on Debian, RHEL, and Arch-family Linux distributions.
-   **FR-002**: The role MUST be idempotent.
-   **FR-003**: The role MUST ensure the `podman.socket` service (or equivalent) is enabled and running.
-   **FR-004**: The role MUST allow configuration of the `registries.search` list via a `registries_search` variable.
-   **FR-005**: The role MUST allow configuration of the `registries.insecure` list via a `registries_insecure` variable.
-   **FR-006**: The role MUST allow configuration of the `registries.block` list via a `registries_block` variable.
-   **FR-007**: The role MUST fail gracefully with a clear error message if run on an unsupported operating system.
-   **FR-008**: The role MUST validate the format of provided registry URIs. If a URI is invalid, the role MUST fail the Ansible run and provide a clear error message.

### Non-Functional Requirements

-   **NFR-001**: The role MUST document all input variables in the `README.md` file, including their name, description, and default value.

## Assumptions

- The target systems have internet access to download the `podman` package from the official repositories.
- The user running the Ansible playbook has sufficient privileges to install packages and manage services on the target systems.

## Success Criteria *(mandatory)*

### Measurable Outcomes

-   **SC-001**: The Ansible role successfully completes and installs Podman on 100% of targeted, supported, and correctly configured systems.
-   **SC-002**: A system administrator can configure a list of container registries (search, insecure, or block) and have them correctly applied in under 1 minute of playbook execution time.
-   **SC-003**: The role's execution is 100% idempotent, meaning no changes are made on subsequent runs if the desired state is already achieved.