# ansible-basic-structure

This repository is an example and best practice for the basic folder structure of an Ansible project. The repository has no functions, but can be used as a structure diagram for your own projects.

## Preparation

With the following commands, Ansible can be installed on the respective operating system. The installation is not part of this repository.

### Brew
```
brew install git
brew install ansible
ansible --version
```

### Python 
```
sudo easy_install pip
pip install ansible
ansbile --version
```

With the following bash commands the folder structure can be created:

# Folder structure

Create the main folder and go to the folder
```mkdir ansible-basic-project && cd $_ ```

Creates the three folder areas for Development, Staging and Production. In addition, the following two folders playbooks and reds are created in which the respective scripts can be stored.
```mkdir -p inventories/development/{host,group}_vars inventories/staging/{host,group}_vars inventories/production/{host,group}_vars playbooks roles```

Under inventories/ we will manage our inventory (the main topic of this chapter). The *_vars/ folders are used to parameterize single hosts or host groups.
Under playbooks/ we will store playbooks.
Finally, the roles/ folder is for roles.

# ansible.cfg

In addition to the folder structure, we need an ansible.cfg, aa we will probably want to run Ansible with some settings different from the default in the future.

``` touch ansible.cfg && cd $_```

```[defaults]
inventory = inventories/development/inventory # Path to the inventory
private_key_file = ~/.ssh/id-rsa # Path to the SSH key
remote_user = root # User for the SSH connection
host_key_checking = False # Disable host key checking
roles_path = ./roles # Path to the roles
``` 
 
 More Variables and Information: https://docs.ansible.com/ansible/latest/reference_appendices/config.html

# Inventory

The inventory is the central point of Ansible. It is a file that contains all the hosts that Ansible can connect to. The inventory can be a simple text file or a dynamic script that generates the inventory on the fly. The inventory can also be a combination of both.

Add your server with name and ip address to the inventory file. The inventory file is located in the inventories/development/inventory folder.

# Group variables

Group variables are variables that are valid for a group of hosts. For example, if you have a group of web servers, you can define the web server version in the group variables. The group variables are located in the inventories/development/group_vars folder.

all

all is a special group that contains all hosts. If you want to define variables that are valid for all hosts, you can define them in the all group variables.

ansible_user

ansible_user is a special variable that defines the user for the SSH connection. If you want to define a user for all hosts, you can define it in the all group variables.


# Host variables

Host variables are variables that are valid for a single host. For example, if you have a web server that has a different version than the other web servers, you can define the web server version in the host variables. The host variables are located in the inventories/development/host_vars folder.

# Ansible Ping

Ansible Ping is a simple test to see if Ansible can connect to the hosts in the inventory. The ping module is used for this. The ping module is a module that is included in Ansible by default. The ping module sends a ping to the host and waits for a pong. If the host responds with a pong, the ping is successful.

without inventory
```ansible all -m ping```

with inventory
```ansible -i inventories/development/inventory all -m ping```

# Root test

```ansible all -a "head -1 /etc/shadow"```

# -i alternative inventory

Of course, there can be only one default inventory (which you specified in ansible.cfg), but for almost every Ansible command you can specify an alternative inventory with the -i switch.

```ansible -i inventories/development/inventory all -a "head -1 /etc/shadow"```

# More ad-hoc commands

```ansible all -m command -a "df -h /"```
```ansible all -a "hostname" -o```