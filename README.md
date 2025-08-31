# MERN Stack Ansible Project

This repository contains Ansible playbooks and roles for deploying and automation a MERN (MongoDB, Express.js, React, Node.js) stack application.

## Roles Overview

- **`Comman`**: Handles basic system setup, common package installations (e.g., `ntp`, `git`, `vim`), user and group management, and SSH key deployment.
- **`Dbserver`**: Installs and configures MongoDB, including repository setup, package installation, data directory creation, and initial user setup.
- **`Webserver`**: Sets up the Node.js environment, deploys a React application, installs npm packages, and manages the application's lifecycle.

## Usage

To use this Ansible project, you will typically:

1.  **Configure your inventory**: Update `inventory/hosts` with your target server details.
2.  **Adjust variables**: Modify variables in `group_vars/` or role-specific `defaults/main.yml` as needed.
3.  **Run the main playbook**: Execute `ansible-playbook playbook.yml` to deploy the entire MERN stack.

project Tree  

```
.
├── ansible.cfg
├── ansible-multipass.yaml
├── commands
├── group_vars
│   ├── all.yml
│   └── webserver.yaml
├── inventory
│   └── hosts
├── playbook.yml
└── roles
    ├── comman
    │   ├── defaults
    │   │   └── main.yml
    │   ├── files
    │   ├── handlers
    │   │   └── main.yml
    │   ├── meta
    │   │   └── main.yml
    │   ├── README.md
    │   ├── tasks
    │   │   └── main.yml
    │   ├── templates
    │   ├── tests
    │   │   ├── inventory
    │   │   └── test.yml
    │   └── vars
    │       └── main.yml
    ├── dbserver
    │   ├── defaults
    │   │   └── main.yml
    │   ├── files
    │   ├── handlers
    │   │   └── main.yml
    │   ├── meta
    │   │   └── main.yml
    │   ├── README.md
    │   ├── tasks
    │   │   ├── configure.yaml
    │   │   ├── install.yaml
    │   │   └── main.yml
    │   ├── templates
    │   │   ├── mongod.conf.j2
    │   │   └── mongoshrc.js.j2
    │   ├── tests
    │   │   ├── inventory
    │   │   └── test.yml
    │   └── vars
    │       └── main.yml
    └── webserver
        ├── defaults
        │   └── main.yml
        ├── files
        │   └── App.css
        ├── handlers
        │   └── main.yml
        ├── meta
        │   └── main.yml
        ├── README.md
        ├── tasks
        │   └── main.yml
        ├── templates
        │   └── App.js.j2
        ├── tests
        │   ├── inventory
        │   └── test.yml
        └── vars
            └── main.yml

31 directories, 37 files
```


 For detailed information on each role, refer to their respective `README.md` files located in `roles/<role_name>/README.md`.


## License

This project is licensed under the MIT-0 License and Owner to Omar Khaled Tokal
