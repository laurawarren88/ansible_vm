# **Ansible Project for Network Configuration and Services Deployment**

## Overview
This Ansible project automates the configuration of a network of VMs which includes a:
```
Gateway
DNS
DHCP
web server
```
The playbooks facilitate the setup of these virtual machines in ***vSphere***.

## Prerequisites 
This project assumes you have the following: 
```
- Access to a vSphere environment
- Necessary permissions for VM management and configuration
- Have Python3.X Installed
```

## Step One
Change your directory to where you wish to run this script and store the cloned repository:
```bash
cd <filename>
```

## Step Two
Clone the repository from github and then you can set your python virtual environment
```bash
git clone https://github.com/laurawarren88/ansible_vm.git
```

## Step Three 
Now you have the repository stored where you want it you can set your python virtual environment and install all the necessary requirments
```bash
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
```

## Step Four 
Take a look around the file structure and see what is happening. 
You will need to add a secrets file into the file tree to store necessary information to run the playbook. 
I'll walk you through it:
```bash
touch roles/vars/secrets.yml
vim roles/vars/secrets.yml
```

In the file you need to include your information into the following variables:
```
vars_vcenter_username: ""
vars_vcenter_password: ""
vars_folder_path: ""
vars_internal_network: ""
```

Inbetween the double quotes store your username and password you use to login to vSphere. 

For the folder path list a location where you want the VMs to be stored in vSphere e.g. "/Development/vm".

For the internal network, list the name of a VM network you want to use for the internal network to connect to. 

Once you have these set make sure you save the file and encrypt it, remember the password you set you'll need it later 😜. 
```bash
ansible-vault encrypt roles/vars/secrets.yml
```

## Step Five 
Inorder to be able to set up and use the VMs from your machine you need to generate an SSH key pair.

Run the following command to create a new SSH key:
```bash
ssh-keygen 
```
When prompted save the file in the default location (~/.ssh/id_rsa) as this is what is defined in the inventory, if you do wish to use your own location make sure you make the necessary changes to the inventory.ini file.

You may also be asked for a passphrase. You can choose to set one or leave it empty.

Once you have generated your keys you will need to add your public key to the authorized keys file on each VM. 
```bash
ssh-copy-id root@<vm_ip>
```
Repeat this step for each VM. IPs are set in the roles/vars/main.yml file. 

## Step Six
Run the Playbook: 
```bash
ansible-playbook -i inventory.ini main.yml
```

If using encrypted files, run the playbook with: 
```bash
ansible-playbook -i inventory.ini main.yml --ask-vault-pass
```

## Step Seven
Testing the Setup:
```bash
curl http://<webserver_ip>:3000 | jq
```