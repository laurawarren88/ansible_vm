# **Ansible Project for Network Configuration** 💻

## ⭐️ Overview
This Ansible project automates the configuration of a network of VMs which includes:
```
🔹 Gateway
🔹 DNS
🔹 DHCP
🔹 Webserver
```
The playbooks facilitate the setup of these virtual machines in [vSphere](https://vcenter.easlab.co.uk).

## ⚙️ Prerequisites 
This project assumes you have the following: 
```
🔸 Access to a vSphere environment
🔸 Necessary permissions for VM management and configuration
🔸 Have Python3.X installed
🔸 Have VS code installed
🔸 Be logged into vSphere
🔸 Have the necessary permissions to download the git repo used in this playbook
🔸 Know how to generate a giuthub token 
```

## 🐾 Step One
Change your directory to where you wish to run this script and store the cloned repository:
```bash
cd <filename>
```

## 🐾 Step Two
Clone the repository from github and then move into the new directory.
```bash
git clone https://github.com/laurawarren88/ansible_vm.git
cd ansible_vm
```

## 🐾 Step Three 
Now you have the repository stored where you want it you can set your python 🐍 virtual environment and install all the necessary requirments
```bash
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
```
When running downloading all the packages it may ask you to update some, feel free to do so. 

## 🐾 Step Four 
Take a look 👀 around the file 📂 structure and see what is happening with VS code. 
```bash
code .
```

You will need to add a secrets 🤫 file into the file tree to store necessary information to run the playbook. 

I'll walk you through it:
```bash
touch roles/vars/secrets.yml
vim roles/vars/secrets.yml
```

In the file you need to include your information ℹ️ into the following variables:
```
vars_vcenter_username: ""
vars_vcenter_password: ""
vars_folder_path: ""
vars_internal_network: ""
vars_email: ""
vars_github_token: ""
```

Inbetween the double quotes store your username and password you use to login to **vSphere**. 

For the folder path list a location where you want the VMs to be stored in vSphere ***e.g. "/Development/vm"***.

For the internal network, list the name of a VM network you want to use for the internal network to connect to. 

The email address is used when generating the key and ease of identification.

Input the generated token from your github generated token.


🤷🏼‍♀️***Optional***🤷🏼‍♀️
Once you have these set make sure you save the file and encrypt it, remember the password 🔐 you set you'll need it later 😜. 
```bash
ansible-vault encrypt roles/vars/secrets.yml
```

## 🐾 Step Five
Run the Playbook, with or without extra information: 
```bash
ansible-playbook -i inventory.ini main.yml -vvvv
```
or 
```bash
ansible-playbook -i inventory.ini main.yml
```

🤷🏼‍♀️***Optional***🤷🏼‍♀️
If using encrypted files, run 🏃🏼‍♀️ the playbook with: 
```bash
ansible-playbook -i inventory.ini main.yml --ask-vault-pass -vvvv
```
or
```bash
ansible-playbook -i inventory.ini main.yml --ask-vault-pass
```

## 🐾 Step Six
Open your browser to vSphere and watch as the VMs are created (in your desired folder). 

Once the VMs are created and powered on:
```
Click the actions dropdown
Click edit setting
Check the box on the network adapter (connected)
```
This will connect the network, seems to be a known bug with vSphere. 

## 🐾 Step Seven
Testing 💯 the Setup:
```bash
curl http://<webserver_ip>:3000 | jq
```
This should return JSON todo list. 