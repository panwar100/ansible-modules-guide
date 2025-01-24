# Ansible Modules Guide

This repository provides a detailed guide to Ansible modules and their usage for automating tasks in IT environments. Each module is explained with practical examples to help you manage systems efficiently.

---

## Table of Contents
1. [Introduction](#introduction)
2. [Core Modules](#core-modules)
    - [Ping Module](#ping-module)
    - [Command Module](#command-module)
    - [Shell Module](#shell-module)
    - [Copy Module](#copy-module)
    - [File Module](#file-module)
    - [User Module](#user-module)
    - [Service Module](#service-module)
    - [Yum Module](#yum-module)
3. [Common Usage Examples](#common-usage-examples)
4. [Best Practices](#best-practices)

---

# Introduction

Ansible modules are the building blocks for automating tasks. They are small, reusable units of code that can be used independently or in playbooks. This guide explains frequently used modules with real-world examples.

---

# Core Modules

## Ping Module
- **Purpose**: Check connectivity to target hosts.
- **Example**:
   ```bash
   ansible all -m ping

Output:

192.168.1.10 | SUCCESS => {
    "ping": "pong"
}

#
## Command Module

- **Purpose**: Run commands on remote hosts (without a shell).
- **Example**:

      ansible all -m command -a "df -h"

Output:

Filesystem  Size  Used  Avail  Use%  Mounted on
/dev/sda1   20G   10G   8G     50%   /

#
## Shell Module
- **Purpose**: Execute commands through a shell on remote hosts.
- **Example**:
 
       ansible all -m shell -a "echo $HOME"

Difference from Command Module: Supports shell features like variables, pipes, and redirects.


#
## Copy Module
- **Purpose**: Copy files from the control machine to target hosts.
- **Example**:

      ansible all -m ansible.builtin.copy -a "src=/etc/hosts dest=/tmp/hosts"

#
## File Module
- **Purpose**: Manage file and directory permissions, create or delete files/directories.
- **Examples**:
   - **Create a Directory**:

          ansible all -m file -a "dest=/tmp/new_directory state=directory mode=0755"

   - **Change File Permissions**:
    
         ansible all -m file -a "dest=/tmp/sample.txt mode=0644"


#
## User Module
- **Purpose**: Manage user accounts on target systems.
- **Examples**:
  - **Add a User**:

        ansible all -m user -a "name=test_user password=<encrypted_password>"

  - **Delete a User**:

        ansible all -m user -a "name=test_user state=absent"


#
## Service Module
- **Purpose**: Manage system services (start, stop, enable, disable).
- **Examples**:
  - **Start a Service**:

        ansible all -m service -a "name=httpd state=started"

  - **Enable a Service**:

        ansible all -m service -a "name=httpd enabled=yes"


#
## Yum Module
- **Purpose**: Install, update, or remove packages on RPM-based systems.
- **Examples**:
  - **Install a Package**:

        ansible all -b -m yum -a "name=httpd state=present"

  - **Remove a Package**:

        ansible all -b -m yum -a "name=httpd state=absent"
    
  - **Update a Package**:

        ansible all -b -m yum -a "name=httpd state=latest"

---

# Common Usage Examples
1. **Create a File**:

       ansible all -m shell -a "touch /tmp/testfile"

2. **Check Disk Usage**:

       ansible all -m command -a "df -h"

3. **Copy and Set Permissions**:

       ansible all -m copy -a "src=/etc/hosts dest=/tmp/hosts mode=0644"

4. **Restart a Service**:

       ansible all -m service -a "name=httpd state=restarted"

---

# Best Practices

1.Use **playbooks** for complex workflows to make tasks reusable and structured.

2.Leverage **inventory groups** for better host management.

3.Avoid hardcoding credentials; use **Vault** for secure secrets management.
