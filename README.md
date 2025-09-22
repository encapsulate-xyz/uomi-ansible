# Ansible Playbook to deploy Uomi Node

This Ansible playbook automates the deployment and configuration of Uomi Node. It ensures that the necessary dependencies, configuration files, and services are properly set up and running.

## Table of Contents

- [Requirements](#requirements)
- [Setup](#setup)
- [Variables](#variables)
- [Usage](#usage)

## Requirements

Before using this playbook, ensure the following requirements are met:

1. **Ansible version**: Make sure you have Ansible 2.15+ installed.
2. **SSH Access**: Passwordless SSH access to all target servers.
3. **Python**: Python 3.x installed on the control node and all target hosts.
4. **Privileges**: The user running the playbook must have sudo privileges on the target machines.

**Note**: The following ansible playbook dynamically fetches private validator and node keys from hashicorp vault. 

## Setup

### 1. Install Ansible

If Ansible is not installed, visit the official documentation for detailed instructions on how to install Ansible on various Linux distributions:

[Ansible Installation Guide](https://docs.ansible.com/ansible/latest/installation_guide/installation_distros.html)


### 2. Clone the repository

Clone this repository to your Ansible control node:

```bash
git clone https://github.com/encapsulate-xyz/uomi-ansible.git
cd uomi-ansible
```

### 3. Inventory

Define your target servers' IP address or DNS in the inventory folder, and select either `mainnet.yml` or `testnet.yml` to update.

Example for testnet.yml

```yaml
---
# maintains testnet inventory grouped by project names
all:
  vars:
    env: testnet
  children:
    uomi:
      hosts:
        validator.uomi.testnet.encapsulate.xyz:
          type: validator
        ai.uomi.testnet.encapsulate.xyz:
          type: ai
```

## Variables

This playbook allows customization through several variables. You can define these variables in the following locations:

- **`group_vars/all.yml`**: Contains all the source urls and configurations.
- **`group_vars/mainnet.yml`** or **`group_vars/testnet.yml`**: Contains version-specific variables.
- **`group_vars/vault.yml`**: Store secret variables, such as `jwtsecret`, in this file.

### Usage

1. First, install the dependencies:

  ```bash
   ansible-galaxy install -r requirements.yml
  ```

2. Create a `ansible_vault_password` file containing ansible-vault password

3. Then run the playbook:

 - To setup node:

  ```bash
  ansible-playbook setup_node.yml -l validator.uomi.testnet.encapsulate.xyz
  ```
 - To setup AI Service:
 
  ```bash
  ansible-playbook setup_ai.yml -l ai.uomi.testnet.encapsulate.xyz
  ```

After you run the playbook, it will ask for confirmation, displaying all the variables and the IP address or DNS of the server you are going to deploy.

Example output:

```bash
TASK [Display environment being deployed to] ***************************************************************************************************
ok: [validator.uomi.testnet.encapsulate.xyz] => {
    "msg": [
        "Deploying to Host: validator.uomi.testnet.encapsulate.xyz",
        "Groups: ['uomi']",
        "Project: uomi",
        "Environment: testnet",
        "Type: validator",
        "Version: v0.3.3",
        "Username: uomi",
        "Service Name: uomi",
        "Operating System: linux",
        "System Architecture: amd64"
    ]
}

TASK [Confirm deployment details] ********************************************************************************************************************
Pausing for 40 seconds
(ctrl+C then 'C' = continue early, ctrl+C then 'A' = abort)
ok: [validator.uomi.testnet.encapsulate.xyz]

TASK [Please confirm again] ********************************************************************************************************************
ok: [validator.uomi.testnet.encapsulate.xyz] => {
    "msg": [
        "Deploying to Host: validator.uomi.testnet.encapsulate.xyz",
        "Project: uomi",
        "Environment: testnet",
        "Type: validator"
    ]
}

TASK [Confirm deployment details] **************************************************************************************************************
Pausing for 20 seconds
(ctrl+C then 'C' = continue early, ctrl+C then 'A' = abort)
ok: [validator.uomi.testnet.encapsulate.xyz]
```
