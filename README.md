# MernSible – Ansible Automation for MERN Stack

**MernSible** is an Ansible-based automation project that streamlines the deployment of a complete **MERN (MongoDB, Express, React, Node.js)** environment.  
It is built around three modular roles:  

- **Comman** → standard system setup and user management  
- **DBserver** → automated MongoDB deployment and configuration  
- **Webserver** → Node.js & React application setup and deployment  

This structure ensures **consistency, scalability, and security** across environments, making it ideal for development, testing, or production-ready deployments.  

---

## 📌 Project Overview

MernSible simplifies the often complex process of provisioning and configuring infrastructure for modern web applications.  
With just a few Ansible playbooks, you can:  

- Standardize server setup (users, SSH keys, common packages).  
- Deploy a fully configured MongoDB database with authentication.  
- Launch a Node.js + React web application, managed by **pm2**.  

The modular roles allow flexibility—use them individually or together depending on your project needs.  

---

## 🚀 Roles

### 1. Comman Role
Handles **baseline server setup**:  
- Updates packages and installs essential tools.  
- Creates users, groups, and SSH key pairs.  
- Ensures a secure and consistent configuration across all nodes.  

---

### 2. DBserver Role
Automates **MongoDB deployment and security**:  
- Installs MongoDB from the official repository.  
- Configures data directory, bind IP, and ports.  
- Creates root users with credentials.  
- Enables authentication for production-ready security.  

---

### 3. Webserver Role
Prepares and runs the **Node.js + React application stack**:  
- Installs Node.js, npm, and required global packages.  
- Creates and configures the app directory.  
- Bootstraps a React app with `create-react-app`.  
- Starts and manages the app with **pm2** for reliability.  
- Supports custom pages for branding or business needs.  

---

## 📂 Project Structure

```text
MernSible/
├── ansible.cfg
├── inventory/
│   └── hosts
├── playbook.yml
├── roles/
│   ├── comman/
│   │   ├── tasks/
│   │   ├── defaults/
│   │   ├── vars/
│   │   ├── handlers/
│   │   └── README.md
│   ├── dbserver/
│   │   ├── tasks/
│   │   ├── templates/
│   │   ├── defaults/
│   │   └── README.md
│   └── webserver/
│       ├── tasks/
│       ├── templates/
│       ├── defaults/
│       └── README.md
└── group_vars/
    └── all.yml


---

## ✅ Requirements

- **Ansible 2.9+** installed on the control node.  
- Target servers running **Ubuntu/Debian**.  
- **Python** installed on target machines.  
- Internet access for package installation.  

---

## 🔑 Why MernSible?

- **Reusable** – modular roles can be applied independently.  
- **Scalable** – easily extend to multiple servers with inventory groups.  
- **Secure** – SSH key management & MongoDB authentication.  
- **Automated** – end-to-end MERN environment with minimal manual setup.  

---

## 📜 License

This project is licensed under the MIT-0 License.  

