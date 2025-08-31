# dbserver Role

This README provides comprehensive documentation for the `dbserver` Ansible role, which automates the deployment and configuration of a MongoDB database server. This role is designed to be idempotent and handles the installation of MongoDB, its dependencies, and the initial configuration, including user creation and security settings.


## Overview

The `dbserver` role is responsible for setting up a MongoDB instance on a target server. It ensures that all necessary packages are installed, the MongoDB service is running and enabled, and basic security configurations are applied. The role is flexible, allowing customization of MongoDB version, bind IP, and user credentials through Ansible variables.

## Requirements

This role requires:

- Ansible 2.9 or higher.
- Target server running Ubuntu (specifically tested with versions compatible with MongoDB's official APT repository).
- Python 3 and `python3-pip` on the target server for `pymongo` and `configparser` installation, which are used by Ansible's `mongodb_user` module.
- Internet connectivity on the target server to download MongoDB packages and dependencies.

## Role Variables

Variables for this role can be defined in `defaults/main.yml`, `vars/main.yml`, or `group_vars/all.yml` (or other group/host variables files). Below are the key variables and their descriptions:

| Variable Name           | Default Value         | Description                                                                 |
| :---------------------- | :-------------------- | :-------------------------------------------------------------------------- |
| `mongodb_version`       | `8.0`                 | The version of MongoDB to install.                                          |
| `mongodb_repo_key`      | `https://www.mongodb.org/static/pgp/server-{{ mongodb_version }}.asc` | URL for the MongoDB APT repository GPG key.                                 |
| `mongodb_repo_url`      | `https://repo.mongodb.org/apt/{{ ansible_distribution | lower }}/{{ ansible_distribution_release | lower }}/mongodb-org/{{ mongodb_version }}` | URL for the MongoDB APT repository.                 |
| `mongodb_port`          | `27017`               | The port on which MongoDB will listen.                                      |
| `mongodb_bind_ip`       | `0.0.0.0`             | The IP address(es) MongoDB will bind to. `0.0.0.0` binds to all available network interfaces. |
| `mongodb_config_path`   | `/etc/mongod.conf`    | The path to the MongoDB configuration file.                                 |
| `mongodb_data_path`     | `/var/lib/mongodb`    | The path to the MongoDB data directory.                                     |
| `mongodb_root_user`     | `root`                | The username for the MongoDB root user to be created.                       |
| `mongodb_root_password` | `password`            | The password for the MongoDB root user. **Change this in production!**      |

## Dependencies

This role does not explicitly depend on other custom Ansible roles. However, it relies on several Ansible modules and system packages:

- `ansible.builtin.apt`: For managing APT packages.
- `ansible.builtin.file`: For managing file system paths and permissions.
- `ansible.builtin.service`: For managing the MongoDB service.
- `ansible.builtin.template`: For templating configuration files.
- `ansible.builtin.deb822_repository`: For adding the MongoDB APT repository.
- `pip`: For installing Python packages like `pymongo` and `configparser`.
- `mongodb_user` (from `community.mongodb` collection, implicitly used if `pymongo` is installed): For managing MongoDB users.

## Example Playbook

To use this role, include it in your playbook. Here's a basic example:

```yaml
---
- name: Deploy MongoDB Server
  hosts: dbservers
  become: true
  roles:
    - dbserver
  vars:
    mongodb_root_password: your_secure_password
```

Make sure your `inventory/hosts` file defines the `dbservers` group with the appropriate target hosts.

## Tasks Breakdown

The `dbserver` role's main tasks are orchestrated through `tasks/main.yml`, which includes two primary sub-tasks:

### `install.yaml`

This file handles the installation of MongoDB and its prerequisites:

1.  **Install `python3-pip`**: Ensures `pip` is available for Python package management.
2.  **Remove `/usr/lib/python3.12/EXTERNALLY-MANAGED`**: Allows `pip` to install packages globally, bypassing Python's external management mechanism.
3.  **Install `pymongo` and `configparser`**: These Python libraries are essential for Ansible to interact with MongoDB, particularly for user management.
4.  **Add MongoDB repository**: Configures the official MongoDB APT repository using `deb822_repository` for the specified `mongodb_version`.
5.  **Update APT package cache**: Refreshes the package list after adding the new repository.
6.  **Install MongoDB packages**: Installs `mongodb-org` which includes the MongoDB server, shell, and other tools.
7.  **Ensure MongoDB is enabled and started**: Configures the `mongod` service to start on boot and ensures it is currently running.

### `configure.yaml`

This file manages the configuration of the MongoDB instance:

1.  **Create MongoDB data directory**: Ensures the `mongodb_data_path` exists with correct ownership and permissions.
2.  **Template MongoDB configuration file**: Renders `mongod.conf.j2` to `mongodb_config_path` (`/etc/mongod.conf` by default), applying variables like `mongodb_bind_ip` and `mongodb_port`. This task notifies the `Restart MongoDB` handler upon changes.
3.  **Check for `.mongorc.js`**: Determines if a `.mongorc.js` file exists for the root user, which is used to prevent re-creation of the root user.
4.  **Create MongoDB root user**: Creates a root user with specified `mongodb_root_user` and `mongodb_root_password` if the `.mongorc.js` file does not exist. This ensures the root user is created only once.
5.  **Deploy MongoDB `.mongoshrc.js` file**: Deploys the `mongoshrc.js.j2` template to the root user's home directory. This file can contain shell customizations or initial commands for `mongosh`.
6.  **Enable authentication in MongoDB**: Modifies `/etc/mongod.conf` to enable authentication, adding `security.authorization: enabled`. This task also notifies the `Restart MongoDB` handler.

## Configuration Files

This role primarily manages the following configuration files:

-   `/etc/mongod.conf`: The main MongoDB configuration file, templated from `templates/mongod.conf.j2`.
-   `~/.mongoshrc.js`: A JavaScript file for `mongosh` (MongoDB Shell) customizations, templated from `templates/mongoshrc.js.j2`.

## Usage

To deploy a MongoDB server using this role:

1.  **Prepare your inventory**: Ensure your `inventory/hosts` file includes a `dbservers` group with the IP addresses or hostnames of your target MongoDB servers.

    ```ini
    [dbservers]
    your_db_server_ip_1
    your_db_server_ip_2
    ```

2.  **Create a playbook**: Create a playbook (e.g., `deploy_db.yml`) that includes the `dbserver` role.

    ```yaml
    ---
    - name: Deploy MongoDB Server
      hosts: dbservers
      become: true
      roles:
        - dbserver
      vars:
        mongodb_root_password: your_secure_password_here
    ```

3.  **Run the playbook**: Execute the playbook using `ansible-playbook`:

    ```bash
    ansible-playbook -i inventory/hosts deploy_db.yml
    ```

    Remember to replace `your_secure_password_here` with a strong password for your MongoDB root user.

## Author

Manus AI

## License
