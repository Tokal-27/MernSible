# Webserver Role

This README provides comprehensive documentation for the `Webserver` Ansible role, which automates the deployment and configuration of a web server, specifically a Node.js application with a React frontend.


## Overview

The `webserver` role is designed to set up a Node.js environment, deploy a React application, and ensure it is running correctly. It handles the installation of Node.js, npm, specified Node.js packages, and the creation/configuration of a React application.

## Requirements

This role requires:

- Ansible 2.9 or higher.
- Target server running Ubuntu (specifically tested with versions compatible with Node.js and npm).
- Internet connectivity on the target server to download Node.js, npm, and other specified npm packages.

## Role Variables

Variables for this role can be defined in `defaults/main.yml`, `vars/main.yml`, or `group_vars/all.yml` (or other group/host variables files). Below are the key variables and their descriptions:

| Variable Name           | Default Value         | Description                                                                 |
| :---------------------- | :-------------------- | :-------------------------------------------------------------------------- |
| `app_dir`               | `var/www/mern-app`    | The directory where the application will be deployed.                       |
| `npm_packages`          | `['express', 'create-react-app', 'pm2']` | A list of npm packages to install globally.                                 |
| `app_name`              | `my_app`              | The name of the React application to be created.                            |
| `deploy_custom_page`    | `false`               | If true, custom `App.css` and `App.js` files will be deployed.              |

## Dependencies

This role does not explicitly depend on other custom Ansible roles. However, it relies on several Ansible modules and system packages:

- `ansible.builtin.apt`: For managing APT packages.
- `ansible.builtin.file`: For managing file system paths and permissions.
- `ansible.builtin.command`: For executing shell commands.
- `ansible.builtin.npm`: For managing npm packages.

## Example Playbook

To use this role, include it in your playbook. Here's a basic example:

```yaml
---
- name: Deploy Web Server
  hosts: webservers
  become: true
  roles:
    - webserver
  vars:
    app_dir: /opt/my-web-app
    npm_packages:
      - express
      - pm2
    app_name: my_react_app
    deploy_custom_page: true
```

Make sure your `inventory/hosts` file defines the `webservers` group with the appropriate target hosts.

## Tasks Breakdown

The `webserver` role's main tasks are orchestrated through `tasks/main.yml`, which includes the following primary sub-tasks:

1.  **Install Node.js and npm**: Ensures `nodejs` and `npm` are installed on the target server using `apt`.
2.  **Create the app directory**: Creates the specified application directory (`app_dir`) with appropriate ownership and permissions.
3.  **Install npm packages**: Installs a list of global npm packages defined in `npm_packages` using the `npm` module.
4.  **Create React app**: Executes `npx create-react-app` to scaffold a new React application within the `app_dir`.
5.  **Configure React app to listen on 0.0.0.0**: Modifies the `.env` file within the React app directory to ensure the application listens on all network interfaces, which is crucial for external access.
6.  **Deploy custom page**: If `deploy_custom_page` is true, copies custom `App.css` and `App.js` files to the React app's source directory, allowing for custom branding and content.
7.  **Start the Development server**: Initiates the React application using `pm2` for process management, ensuring it runs continuously.

## Configuration Files

This role primarily manages the following configuration files:

-   `.env` within the React application directory: Configured to set `HOST=0.0.0.0` for network accessibility.
-   `App.css` and `App.js`: Custom versions of these files can be deployed to override default React app styling and behavior.

## Usage

To deploy a web server using this role:

1.  **Prepare your inventory**: Ensure your `inventory/hosts` file includes a `webservers` group with the IP addresses or hostnames of your target web servers.

    ```ini
    [webservers]
    your_web_server_ip_1
    your_web_server_ip_2
    ```

2.  **Create a playbook**: Create a playbook (e.g., `deploy_web.yml`) that includes the `webserver` role.

    ```yaml
    --- 
    - hosts: webservers
      become: true
      roles:
        - webserver
      vars:
        app_dir: /opt/my-web-app
        npm_packages:
          - express
          - pm2
        app_name: my_react_app
        deploy_custom_page: true
    ```

3.  **Run the playbook**: Execute the playbook using `ansible-playbook`:

    ```bash
    ansible-playbook -i inventory/hosts deploy_web.yml
    ```


## License

This project is licensed under the MIT-0 License. See the `README.md` in the root of the Ansible project for more details.
