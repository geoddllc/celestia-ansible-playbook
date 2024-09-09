# Celestia Validator Node Ansible Playbook

## Overview
This Ansible playbook automates the setup and upgrade process for a Celestia node. By default, the configuration assumes the use of an Ubuntu server, with SSH key-based authentication for access.

### Features:
- Automated installation of Go, Celestia binaries, and dependencies.
- Configuration of Celestia node parameters such as ports, peers, and seeds.
- Version-controlled upgrades of the Celestia app.
- Backup and failover to be added (TODO).

## Getting Started

### Prerequisites
- Ansible must be installed on your local machine.
- Ensure that your target servers have Ubuntu installed and are accessible via SSH using a key (not a password).
- Your `hosts.ini` file should define your inventory of servers.

### Hosts Configuration
By default, the playbook uses `ubuntu` as the username for SSH access. Ensure that the user has appropriate privileges for executing commands. The playbook uses SSH key-based authentication by default.

You can modify the `hosts.ini` file to reflect your server configuration:

Example `hosts.ini`:
```ini
[primary]
primary-server ansible_host=172.16.0.18
; with password example
; primary-server ansible_host=172.16.0.18 ansible_ssh_pass=your_password_here

[secondary]
secondary-server ansible_host=192.168.100.2

; If you need to local deployment
; [local]
; localhost ansible_connection=local
```

## Change as Needed:
- Update `ansible_user` if you are using a different username.
- Set the `ansible_ssh_private_key_file` to the path of your SSH key. (Optional)
- Add or remove hosts based on your infrastructure setup.

## Setup Node
To run the playbook, use the following command:
```
ansible-playbook -i hosts.ini playbooks/setup_celestia.yml -l primary
```
This command will set up the Celestia node on the primary-server. If you want to target multiple servers, you can modify the -l flag to include the desired hosts.

## Upgrade Node 
To upgrade the Celestia nodes, use the following playbook. 
Change the varriable upgrade_clestia.yml to desired version
`` app_version: "v2.1.2"``

```
ansible-playbook -i hosts.ini playbooks/upgrade_celestia.yml
```
This will do automated upgrades of all your servers if needed

## TODO (Planned Features)
- **State Sync**: Implement state sync to improve recovery and reduce sync time.
- **Backup Configuration**: Automatically back up the Celestia configuration files to a cloud service such as S3 or Backblaze.
- **Active-Passive Failover**: Set up an automatic failover system where a secondary node takes over if the primary node fails.

