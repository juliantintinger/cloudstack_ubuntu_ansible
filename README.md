# ansible-cloudstack-ubuntu-aio

[Ansible](http://ansible.com) Playbook to install [Apache CloudStack](cloudstack.apache.org) 4.19 in an All-in-one (aio) setup on Ubuntu 22.04 LTS (KVM, NFS Server, MySQL).

## Components

- MySQL 8.0
- NFS Server (with NFSv4 support)
- KVM/Libvirt
- CloudStack Management Server
- CloudStack KVM Agent

## Prerequisites

- Ubuntu 22.04 LTS server with:
  - Minimum 4GB RAM
  - Minimum 50GB storage
  - One network interface
  - Python 3 installed
  - Ansible 2.9 or higher

## Configuration

Check and modify variables in `group_vars/env.yml`:
- Database passwords
- NFS configuration
- Network settings
- SSH root access settings

## Installation

```bash
ansible-playbook -v -s -i ansible_hosts -e host=<IP_hostname> -e user=ubuntu cs-aio-deploy.yml
```

## Post Installation

Access CloudStack UI at:
http://<server-ip>:8080/client

Default credentials:
- Username: admin
- Password: password

## Features

- Automated installation of all CloudStack components
- Configures MySQL 8.0 with optimized settings
- Sets up NFSv4 for primary and secondary storage
- Configures KVM hypervisor with optimized settings
- Network bridge configuration using netplan
- Systemd service management
- Modern time synchronization with chrony

## Notes

- This playbook is designed for Ubuntu 22.04 LTS
- Uses CloudStack 4.19 repositories
- Configured for KVM hypervisor
- Uses modern networking with netplan
- Optimized for production use


The playbook can now be deployed using:
ansible-playbook -v -s -i ansible_hosts -e host=<IP_hostname> -e user=ubuntu cs-aio-deploy.yml
