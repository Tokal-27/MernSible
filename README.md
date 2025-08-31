# MERN Stack Ansible Project

This repository contains Ansible playbooks and roles for deploying a MERN (MongoDB, Express.js, React, Node.js) stack application.

## Project Structure

- `ansible-multipass.yaml`: Playbook for setting up virtual machines using Multipass.
- `ansible.cfg`: Ansible configuration file.
- `commands`: A file that might contain useful commands or scripts.
- `group_vars/`: Directory containing variable definitions for groups of hosts.
  - `all.yml`: Variables applicable to all hosts.
  - `webserver.yaml`: Variables specific to webserver hosts.
- `inventory/`: Contains the Ansible inventory file.
  - `hosts`: Defines the hosts and groups for the Ansible deployment.
- `playbook.yml`: The main Ansible playbook for deploying the MERN stack.
- `roles/`: Directory containing individual Ansible roles.
  - `comman/`: Role for common system configurations and package installations.
  - `dbserver/`: Role for deploying and configuring MongoDB.
  - `webserver/`: Role for deploying and configuring the Node.js/React web application.

## Roles Overview

- **`Comman`**: Handles basic system setup, common package installations (e.g., `ntp`, `git`, `vim`), user and group management, and SSH key deployment.
- **`Dbserver`**: Installs and configures MongoDB, including repository setup, package installation, data directory creation, and initial user setup.
- **`Webserver`**: Sets up the Node.js environment, deploys a React application, installs npm packages, and manages the application's lifecycle.

## Usage

To use this Ansible project, you will typically:

1.  **Configure your inventory**: Update `inventory/hosts` with your target server details.
2.  **Adjust variables**: Modify variables in `group_vars/` or role-specific `defaults/main.yml` as needed.
3.  **Run the main playbook**: Execute `ansible-playbook playbook.yml` to deploy the entire MERN stack.

For detailed information on each role, refer to their respective `README.md` files located in `roles/<role_name>/README.md`.


##  author: Omar Khaled Tokal
## description: Cloud Devops Enginner 

## License

This project is licensed under the MIT-0 License.
