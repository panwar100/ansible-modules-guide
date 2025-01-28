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

### General Syntax of an Ansible Ad-Hoc Command:
```
ansible <host-pattern> -b -m <module-name> -a "<module-arguments>"
```

### Explanation of Each Component:
**1. ansible**:
The command-line tool used to run ad-hoc commands or tasks on managed nodes.

**2.` <host-pattern>`**:

This specifies the target group of hosts or a single host on which the command will run.

Example:

all: Run on all hosts in your inventory.

web: Run on a group of hosts named "web".

hostname: Run on a specific host.(e.g.**demo** use in below content)

**3. -b (short for --become)**:

Enables privilege escalation (similar to sudo in Linux). This is needed when performing tasks that require administrative privileges.

**4. -m `<module-name>`**:

Specifies the module to use. A module is like a function in Ansible that performs a specific task.
In your example, group is the module for managing system groups.

**5. -a `"<module-arguments>"`**:

Provides the arguments or options to the module. These arguments control what the module does.

---

# Core Modules

## Ping Module
- **Purpose**: Check connectivity to target hosts.
- **Example**:
   ```bash
   ansible all -m ping

Output:

   ![Screenshot from 2025-01-27 22-41-16](https://github.com/user-attachments/assets/a5f623b3-45c9-4018-a2e7-8b77bdc476de)


#
## Command Module

- **Purpose**: Run commands on remote hosts (without a shell).
- **Example**:

      ansible all -m command -a "df -h"

Output:

 ![Screenshot from 2025-01-27 22-42-37](https://github.com/user-attachments/assets/6f9867cb-7640-4df4-a4e3-c9af1e2c23b7)



#
## Shell Module
- **Purpose**: Execute commands through a shell on remote hosts.
- **Example**:
 
       ansible all -m shell -a "echo $HOME"

     ![Screenshot from 2025-01-27 22-44-00](https://github.com/user-attachments/assets/5d64eff1-cfc1-4704-934a-06096e0692b5)


Difference from Command Module: Supports shell features like variables, pipes, and redirects.


#
## Copy Module
- **Purpose**: Copy files from the control machine to target hosts.
- **Example**:

      ansible all -m ansible.builtin.copy -a "src=/etc/hosts dest=/tmp/hosts"

     ![Screenshot from 2025-01-27 22-45-14](https://github.com/user-attachments/assets/9bc833c5-b9d5-4b37-87da-320e88b5dc52)


#
## File Module
- **Purpose**: Manage file and directory permissions, create or delete files/directories.
- **Examples**:
   - **Create a Directory**:

          ansible all -m file -a "dest=/tmp/new_directory state=directory mode=0755"

        ![Screenshot from 2025-01-27 22-46-37](https://github.com/user-attachments/assets/6101feaa-ee2d-4e87-a208-7c52c33f4579)

   - **Change File Permissions**:
    
         ansible all -m file -a "dest=/tmp/sample.txt mode=0644"

       ![Screenshot from 2025-01-27 22-50-50](https://github.com/user-attachments/assets/ec4e70ee-9d98-490e-bbe7-e80b74d23155)


#
## User Module
- **Purpose**: Manage user accounts on target systems.
- **Examples**:
  - **Add a User**:

        python3 -c 'import crypt,getpass;pw=getpass.getpass();print(crypt.crypt(pw) if (pw==getpass.getpass("Confirm: ")) else exit())'

       
       ![Screenshot from 2025-01-27 23-07-48](https://github.com/user-attachments/assets/a13653c8-34d4-4d1e-a6a4-d4c33e044ab5)

        ansible all -b -m ansible.builtin.user -a "name=test_user password=<encrypted_password>"

       ![Screenshot from 2025-01-27 23-05-36](https://github.com/user-attachments/assets/2a959811-2979-4ec9-8987-54a6460df261)


  - **Delete a User**:

        ansible all -b -m ansible.builtin.user -a "name=test_user state=absent"

       ![Screenshot from 2025-01-27 23-09-44](https://github.com/user-attachments/assets/3cc4b720-3248-4a61-ac72-2ba897d06dd3)

 

#
## Service Module
- **Purpose**: Manage system services (start, stop, enable, disable).
- **Examples**:
  - **Start a Service**:

        ansible all -b -m service -a "name=httpd state=started"

       ![Screenshot from 2025-01-27 23-15-49](https://github.com/user-attachments/assets/10c3ba50-ee53-43e7-afc1-c95ac38fd94d)

  - **Enable a Service**:

        ansible all -b -m service -a "name=httpd enabled=yes"

       ![Screenshot from 2025-01-27 23-18-05](https://github.com/user-attachments/assets/80923292-9295-40b1-91c2-15bbe3ff76f9)


#
## Yum Module
- **Purpose**: Install, update, or remove packages on RPM-based systems.
- **Examples**:
  - **Install a Package**:

       - check httpd:

       ![Screenshot from 2025-01-28 23-30-46](https://github.com/user-attachments/assets/bd46e6d4-5ecc-4f20-a2eb-63919dde66a4)


        ansible all -b -m yum -a "name=httpd state=present"

       ![Screenshot from 2025-01-27 23-11-23](https://github.com/user-attachments/assets/c9c8823d-d07b-4c34-8ee8-efd430e63194)


  - **Remove a Package**:

        ansible all -b -m yum -a "name=httpd state=absent"

       ![Screenshot from 2025-01-27 23-23-51](https://github.com/user-attachments/assets/7c4da9d0-2ba1-47e4-b962-aca98e984232)

    
  - **Update a Package**:

        ansible all -b -m yum -a "name=httpd state=latest"

       ![Screenshot from 2025-01-27 23-19-06](https://github.com/user-attachments/assets/1382acf1-70aa-48f8-9c6f-5434d6988d79)


---

# Common Usage Examples
1. **Create a File**:

       ansible all -m shell -a "touch /tmp/testfile"

      ![Screenshot from 2025-01-27 23-24-35](https://github.com/user-attachments/assets/d343c43f-41ec-4fae-a3c5-57e2d3722f2f)


2. **Check Disk Usage**:

       ansible all -m command -a "df -h"

      ![Screenshot from 2025-01-27 23-25-39](https://github.com/user-attachments/assets/c83eea5b-946b-47f0-9375-b351f3e7bfe0)


3. **Copy and Set Permissions**:

       ansible all -m copy -a "src=/etc/hosts dest=/tmp/hosts mode=0644"

      ![Screenshot from 2025-01-27 23-26-46](https://github.com/user-attachments/assets/b44d9159-48fa-4d31-9e13-5c41d8bd7c59)

      ![Screenshot from 2025-01-28 23-41-42](https://github.com/user-attachments/assets/18b5f5a0-14c3-4071-b398-ff105507df8e)

       
4. **Restart a Service**:

       ansible all -b -m service -a "name=httpd state=restarted"

      ![Screenshot from 2025-01-27 23-29-33](https://github.com/user-attachments/assets/9ff11d9d-b01c-4c41-8070-24a0b72ed2ef)

5. **Groups**:
  
   - 5.0 **Add group**:

         ansible demo -b -m group  -a "name=g1 state=present"

        ![Screenshot from 2025-01-28 22-46-26](https://github.com/user-attachments/assets/472f23ca-ae94-4bb0-9435-869dae753795)
 
    - 5.1 **check group**:
 
           ansible demo -b -m shell -a "cat /etc/group | grep g1"

         ![Screenshot from 2025-01-28 22-50-16](https://github.com/user-attachments/assets/157c059a-3c78-4f9c-888c-df6c6847a0c0)

    - 5.2 **check primary group of user**:

          ansible demo -b -m shell -a "groups testuser1"

         ![Screenshot from 2025-01-28 22-57-12](https://github.com/user-attachments/assets/444977ac-c5a6-4e06-a61f-8e006947524c)

    - 5.3  **Change group**:

      i)add user to primary group
    
          ansible demo -b -m ansible.builtin.user -a "name=testuser1 uid=1003 group=g1"

        ![Screenshot from 2025-01-28 23-06-16](https://github.com/user-attachments/assets/77e111d9-44d9-459c-81a9-9bfa60c9706e)

        ![Screenshot from 2025-01-28 23-08-38](https://github.com/user-attachments/assets/e31fe5b8-15d9-4b6c-887c-29332bb6cfce)

       ii)add user to secondry group

          ansible demo -b -m ansible.builtin.user -a "name=testuser1 groups=g2"
     
         ![Screenshot from 2025-01-28 23-15-36](https://github.com/user-attachments/assets/c176ad77-dbb6-4ae7-8833-4c2564523df5)

        ![Screenshot from 2025-01-28 23-16-49](https://github.com/user-attachments/assets/ffc03eb2-aff3-4dc0-b192-2f2d74fe6f76)

   - 5.4 **Remove group**:

         ansible demo -b -m shell -a "gpasswd -d testuser1 g2"


        ![Screenshot from 2025-01-28 23-19-31](https://github.com/user-attachments/assets/30952e6d-7d18-42d9-9d4b-f353d8f03d8e)
       
---

# Best Practices

1.Use **playbooks** for complex workflows to make tasks reusable and structured.

2.Leverage **inventory groups** for better host management.

3.Avoid hardcoding credentials; use **Vault** for secure secrets management.
