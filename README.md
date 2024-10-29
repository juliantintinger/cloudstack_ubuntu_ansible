# ansible-cloudstack-ubuntu-aio

[Ansible](http://ansible.com) Playbook to install [Apache CloudStack](cloudstack.apache.org) 4.19 in an All-in-one (aio) setup on Ubuntu 22.04 LTS (KVM, NFS Server, MySQL).

## Components

- MySQL 8.0 (Configured with CloudStack-optimized settings)
- NFS Server (with NFSv4 support for primary and secondary storage)
- KVM/Libvirt (Hypervisor)
- CloudStack Management Server 4.19
- CloudStack KVM Agent
- System VM Template 4.19

## Prerequisites

- Ubuntu 22.04 LTS server with:
  - Minimum 4GB RAM
  - Minimum 50GB storage
  - One network interface (configured in env.yml as eth0 by default)
  - Python 3 installed
  - Ansible 2.9 or higher

## Role Structure

- `common/`: Base system configuration
  - System limits configuration
  - Network bridge setup
  - NTP configuration
  - Basic utilities installation
- `cloudstack-manager/`: Management server setup
  - CloudStack repository configuration
  - Java installation (OpenJDK 11)
  - Database initialization
  - System VM template deployment
- `cloudstack-node/`: KVM host configuration
  - KVM and libvirt installation
  - CloudStack agent setup
- `mysql/`: Database configuration
  - MySQL 8.0 installation
  - CloudStack-specific optimizations
  - Database user setup
- `nfs_server/` & `nfs_client/`: Storage configuration
  - NFS v4 setup
  - Primary and secondary storage mounts
- `enable-ssh-root/`: Root SSH access configuration

## Configuration

### Main Configuration File: `group_vars/env.yml`

Key configuration options:
```yaml
# Database Configuration
database_root_password: cloudPass
database_cloud_password: cloudPass

# NFS Configuration
nfs_secondary_storage_host: 127.0.0.1
nfs_domain: domain.net

# Network Configuration
host_interface_device: eth0
host_interface_bridge: cloudbr0
host_dns1: 8.8.4.4
host_dns_domain: domain.net

# CloudStack Repository
cloudstack_repo_url: http://download.cloudstack.org/ubuntu jammy 4.19
url_systemvm_template: http://download.cloudstack.org/systemvm/4.19/systemvmtemplate-4.19.0-kvm.qcow2.bz2

# Security Configuration
host_enable_ssh_direct_root: true
host_root_password: cloudPass
```

## Installation

1. Clone this repository
2. Modify `group_vars/env.yml` according to your environment
3. Update `ansible_hosts` with your target server
4. Run the playbook:



```bash
ansible-playbook -v -b -i ansible_hosts -e host=cirrus -e user=timothy cs-aio-deploy.yml
```
##verbose mode
```bash
ansible-playbook -vvv -b -i ansible_hosts -e host=cirrus -e user=timothy cs-aio-deploy.yml
```

## Post Installation

1. Access CloudStack UI:
   ```
   http://<server-ip>:8080/client
   ```

2. Default credentials:
   - Username: admin
   - Password: password

3. Initial Setup:
   - Change the default password
   - Configure zones, pods, and clusters
   - Add compute resources
   - Configure storage pools
   - Set up networking

## Security Considerations

1. Default Passwords:
   - Change all default passwords defined in `env.yml`
   - Update the CloudStack admin password after first login
   - Consider disabling root SSH access after setup

2. Network Security:
   - The management server runs on port 8080
   - Configure firewall rules accordingly
   - Consider setting up SSL/TLS for the management interface

3. Storage Security:
   - NFS shares should be properly secured
   - Consider implementing storage encryption
   - Regularly backup the MySQL database

## Features

- Automated installation of all CloudStack components
- Configures MySQL 8.0 with optimized settings for CloudStack
- Sets up NFSv4 for primary and secondary storage
- Configures KVM hypervisor with optimized settings
- Network bridge configuration using netplan
- Systemd service management
- Modern time synchronization with chrony
- Extended session timeout (4 hours)
- Optimized system limits for production use

## Troubleshooting

1. Management Server:
   - Logs located at `/var/log/cloudstack/management/`
   - Service status: `systemctl status cloudstack-management`

2. KVM Agent:
   - Logs located at `/var/log/cloudstack/agent/`
   - Service status: `systemctl status cloudstack-agent`

3. MySQL:
   - Logs located at `/var/log/mysql/`
   - Service status: `systemctl status mysql`

## Notes

- This playbook is designed specifically for Ubuntu 22.04 LTS
- Uses CloudStack 4.19 repositories
- Configured for KVM hypervisor only
- Uses modern networking with netplan
- Optimized for production use
- Session timeout is set to 4 hours for management interface
