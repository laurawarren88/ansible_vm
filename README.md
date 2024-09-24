# **Ansible Project for Network Configuration and Services Deployment**

## Overview
This Ansible project automates the configuration of a network including a DNS VM, DHCP VM and a web server. The playbooks facilitate the setup of these virtual machines in vSphere.

## Requirements
•	Ansible (version 2.9 or higher)
•	Access to a vSphere environment
•	Python 3.x
•	Necessary permissions for VM management and configuration
•	SSH keys to SSH into the VMs and to copy the git repository

## Inventory Configuration
The inventory file defines the hosts and their connection parameters. Ensure you update the inventory to reflect your environment.

Playbooks

DNS Server Configuration

The DNS server is configured to handle domain resolution. The playbook installs BIND, sets up the necessary configuration files, and ensures the service is running.

•	Playbook Path: playbooks/dns.yml

DHCP Server Configuration

The DHCP server provides dynamic IP address allocation to devices in the network. This playbook installs the DHCP server software and configures it with the specified settings.

•	Playbook Path: playbooks/dhcp.yml

Web Server Deployment

The web server hosts the application from a specified GitHub repository.

•	Playbook Path: playbooks/webserver.yml

How to Use

1.  Clone the Repository in your desired location. 

2.  Modify the var/main.yml file as you see fit if you want to save the VMs in a new location.

    a.  (Make sure you use the template though as this has the updated mirrorlist and yum repository to deal with the depreciated CentOS)

3. The secrets file contains the username and password of the vSphere account. It also has the file location of the ssh private key to link to the already setup public key on the template from vSphere. 

4. Run the Playbook: ansible-playbook -i inventory.ini main.yml

    a. If using encrypted files, run the playbook with: ansible-playbook -i inventory.ini main.yml --ask-vault-pass

5. Testing the Setup: curl http://<webserver_ip>:3000 | jq
