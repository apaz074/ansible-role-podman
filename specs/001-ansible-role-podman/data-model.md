# Data Model (Role Variables)

This document outlines the user-configurable variables for the `podman` Ansible role.

## Registry Configuration

The role uses three variables to configure Podman's registry settings, corresponding to the sections in `/etc/containers/registries.conf`.

### `registries_search`
- **Type**: `list` of `string`
- **Description**: A list of registries to search for images.
- **Default**: `[]`
- **Example**:
  ```yaml
  registries_search:
    - 'docker.io'
    - 'quay.io'
  ```

### `registries_insecure`
- **Type**: `list` of `string`
- **Description**: A list of registries to be treated as insecure (e.g., using HTTP or having invalid SSL certificates).
- **Default**: `[]`
- **Example**:
  ```yaml
  registries_insecure:
    - 'my-internal-registry.local:5000'
  ```

### `registries_block`
- **Type**: `list` of `string`
- **Description**: A list of registries from which to block image pulling.
- **Default**: `[]`
- **Example**:
  ```yaml
  registries_block:
    - 'untrusted-registry.com'
  ```
