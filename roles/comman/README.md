Role Name = Ansible Role: comman
=========
This role provides common system setup tasks required for the MERN stack project.  
It ensures that essential packages, users, groups, and SSH keys are configured consistently across all servers.

Features
------------
- Updates all system packages to the latest version.
- Installs a list of common packages (`comman_packages`).
- Generates SSH key pairs for specified users.
- Creates user groups and users with desired privileges.
- Adds authorized SSH keys for secure login.
 
Role Variables
--------------

Available variables are defined in `vars/main.yml` and `group_vars/main.yml`:

Example Playbook
----------------
- hosts: all
  become: true
  roles:
    - comman


Dependency 
-----------

```yaml
comman_packages:
  - git
  - curl
  - vim

users:
  - name: mern
    group: mern
    state: present
    key: /tmp/mern_id_rsa
