# Ansible Project for Host Setup

This project uses Ansible to automate the setup and configuration of hosts. It has been structured to follow Ansible best practices, using roles and a main playbook to ensure a reproducible setup.

## Project Structure

- `inventory.yml`: This file defines the hosts that Ansible will manage. You should edit this file to add your target hosts.
- `playbooks/`: This directory contains the main playbooks.
  - `setup.yml`: This is the main playbook for setting up a new host. It includes all the necessary roles.
- `roles/`: This directory contains the Ansible roles. Each role is a self-contained unit of automation.
  - `common`: This role performs common setup tasks, such as configuring package repositories and installing essential packages like `doas`.
  - `authorize_ssh`: This role authorizes an SSH public key for the `root` user, allowing for secure remote access.
- `ansible_legacy/`: This directory contains the original, legacy Ansible project and is kept for reference.

## Getting Started

### 1. Configure the Inventory

Edit the `inventory.yml` file to add your target host's information. You will need to provide the hostname and IP address. You will also need to specify the `ansible_user` that Ansible will use to connect to the host. This user must have passwordless `doas` configured.

Example:
```yaml
all:
  hosts:
    my-server:
      ansible_host: 192.168.1.100
      ansible_user: myuser
      ansible_ssh_private_key_file: ~/.ssh/my-server-key
```

### 2. Configure the SSH Key

The `authorize_ssh` role will authorize a public SSH key for the `root` user. By default, it looks for the key at `~/.ssh/ansible.pub` on the machine where you are running Ansible.

If your key is at a different location, you can override the `authorized_ssh_key_path` variable in the `playbooks/setup.yml` file or by passing it as an extra variable on the command line.

### 3. Run the Playbook

To run the main playbook and set up your host, use the following command:

```bash
ansible-playbook -i inventory.yml playbooks/setup.yml
```

This command will connect to the host specified in your inventory and apply all the roles defined in the `setup.yml` playbook.
