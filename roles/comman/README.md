# Comman Role

This README provides comprehensive documentation for the `comman` Ansible role, which handles common system configurations, package installations, and user management across various servers.

## Overview

The `comman` role is designed to standardize the initial setup of servers by performing essential tasks such as updating packages, installing common utilities, generating SSH key pairs, and managing users and groups. This role ensures a consistent and secure baseline configuration for all managed nodes.

## Requirements

This role requires:

- Ansible 2.9 or higher.
- Target servers running a Debian-based distribution (e.g., Ubuntu) for `apt` package management.
- Python on the target servers.
- Internet connectivity on the target servers to update packages and install new ones.

## Role Variables

Variables for this role can be defined in `defaults/main.yml`, `vars/main.yml`, or `group_vars/all.yml` (or other group/host variables files). Below are the key variables and their descriptions:

| Variable Name     | Default Value                                | Description                                                                 |
| :---------------- | :------------------------------------------- | :-------------------------------------------------------------------------- |
| `comman_packages` | `["ntp", "git", "vim", "unzip", "curl"]` | A list of common packages to install on the target servers.                 |
| `users`           | Defined in `group_vars/all.yml`              | A list of user objects, each containing `name`, `group`, `state`, `path`, and `key` for user and SSH key management. |

## Dependencies

This role does not explicitly depend on other custom Ansible roles. However, it relies on several Ansible modules and system packages:

- `ansible.builtin.apt`: For managing APT packages.
- `community.crypto.openssh_keypair`: For generating SSH key pairs.
- `ansible.builtin.group`: For managing system groups.
- `ansible.builtin.user`: For managing system users.
- `ansible.builtin.authorized_key`: For managing SSH authorized keys.

## Example Playbook

To use this role, include it in your playbook. Here's a basic example:

```yaml
---
- name: Apply Common Configurations
  hosts: all
  become: true
  roles:
    - comman
  vars:
    comman_packages:
      - htop
      - tree
    users:
      - name: newuser
        group: newuser
        state: present
        path: "/home/newuser/.ssh/id_rsa"
        key: "/home/newuser/.ssh/id_rsa"
```

Make sure your `inventory/hosts` file defines the target hosts.

## Tasks Breakdown

The `comman` role's main tasks are orchestrated through `tasks/main.yml`, which includes the following primary sub-tasks:

1.  **Update all packages**: Updates all existing packages on the target server to their latest versions using `apt`.
2.  **Install common packages**: Installs a list of common packages defined in `comman_packages` using `apt`.
3.  **Generate local key pair**: Generates SSH key pairs for specified users on the Ansible control node (delegated to `localhost`).
4.  **Add group**: Creates system groups for specified users.
5.  **Add users to group**: Creates system users and assigns them to their respective groups.
6.  **Add authorized keys**: Adds the public SSH keys to the `authorized_keys` file for each specified user, enabling passwordless SSH access.

## Usage

To apply common configurations using this role:

1.  **Prepare your inventory**: Ensure your `inventory/hosts` file includes the IP addresses or hostnames of your target servers.

    ```ini
    [all]
    your_server_ip_1
    your_server_ip_2
    ```

2.  **Create a playbook**: Create a playbook (e.g., `apply_common.yml`) that includes the `comman` role.

    ```yaml
    ---
    - name: Apply Common Configurations
      hosts: all
      become: true
      roles:
        - comman
      vars:
        comman_packages:
          - htop
          - tree
        users:
          - name: newuser
            group: newuser
            state: present
            path: "/home/newuser/.ssh/id_rsa"
            key: "/home/newuser/.ssh/id_rsa"
    ```

3.  **Run the playbook**: Execute the playbook using `ansible-playbook`:

    ```bash
    ansible-playbook -i inventory/hosts apply_common.yml
    ```

## License

This project is licensed under the MIT-0 
