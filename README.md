<h1 align="center">Virtual Machine Manegement using Ansible</h1>
<div align="center">

![Python version](https://img.shields.io/badge/python-2.7.14+-blue.svg)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

</div>

A project presenting basic management functions of Ansible.

# Ansible

Ansible is an open-source software provisioning, configuration management, and application-deployment tool enabling infrastructure as code. 
It runs on many Unix-like systems, and can configure both Unix-like systems as well as Microsoft Windows.


## Table of contents

1. [Introduction](#introduction)
1. [Installation](#installation)
1. [Usage](#usage)
1. [Next](#next)
1. [Contributing](#contributing)

## Introcustion
This is a project due to the idea of managing multiple virtual machines at once. Managining multiple virtuals machines might a very ardous and inneficient task, 
so Ansible is the right choice to manage all the environment.

## Installation

### Create a sudo user on host machines
Create an arbitrary user with sudo priviliges (eg. `master')

```bash
sudo adduser master
```

Enter visudo

```bash
sudo visudo
```

Add the below line under @sudo

```bash
master   ALL=(ALL:ALL) NOPASSWD:ALL
```


### Generate and copy keys on hosts
Generate private and public keys on your main machine from which you will manage other machines.

```bash
ssh-keygen -t rsa
```

You should find the generated keys under the following direcotry:

```bash
/home/<user>/.ssh/
```

You can copy the generated keys on the host machine using:
```bash
ssh-copy-id -i /home/<user>/.ssh/id_rsa.pub master@<example_ip>
```

### Install Ansible
```bash
sudo apt update
sudo apt install ansible
```

### Clone the repository
```bash
git clone <repository_url>
```


## Usage

Enter the root folder of the repo and configure host information in the `inverntory.yaml` file.
Then run the below command:

```bash
ansible-playbook -i inventory.yaml ./playbooks/prepare.yaml
```

This is the installing log you should see after running the prepare.yaml playbook.

```bash
PLAY [Prepare all host for using ansible scripts] *******************************************

TASK [Kill all apt process and unlock lock apt apt-get] *************************************
fatal: [ansible-vm1]: FAILED! => ...
fatal: [ansible-vm2]: FAILED! => ...

TASK [Remove /var/lib/apt/lists/lock] *******************************************************
fatal: [ansible-vm1]: FAILED! => ...
fatal: [ansible-vm2]: FAILED! => ...

TASK [Remove /var/cache/apt/archives/lock] **************************************************
changed: [ansible-vm1]
changed: [ansible-vm2]

TASK [Remove /var/lib/dpkg/lock] ************************************************************
changed: [ansible-vm1]
changed: [ansible-vm2]

TASK [Reconfigure dpkg] *********************************************************************
changed: [ansible-vm1]
changed: [ansible-vm2]

TASK [Update package] ***********************************************************************
changed: [ansible-vm2]
changed: [ansible-vm1]

TASK [Install needed package] ***************************************************************
changed: [ansible-vm1] => (item=python2.7)
changed: [ansible-vm2] => (item=python2.7)
changed: [ansible-vm1] => (item=python-pip)
changed: [ansible-vm2] => (item=python-pip)

TASK [Update pip] ***************************************************************************
changed: [ansible-vm1]
changed: [ansible-vm2]

PLAY RECAP **********************************************************************************
ansible-vm1                : ok=8    changed=8    unreachable=0    failed=0
ansible-vm2                : ok=8    changed=8    unreachable=0    failed=0
```

After running the above command Python and pip should be installed on the defined hosts.

## Next

Next steps comprise adding new required packages.

## Contributing

Pull requests are welcome. For major changes, please open an issue first to discuss what you would like to change.
