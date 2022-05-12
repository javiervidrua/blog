---
layout: post
title:  "Configuration management with Ansible"
date:   2021-09-21 00:00:00 +0200
categories: jekyll update
---
## Overview

Ansible is a tool that allows us to automate several things:
* Cloud provisioning
* Configuration management
* Application deployment
* Intra-service orchestration

It is built from the ground up for multi-tier deployments, defining how the systems interact, instead of defining what each one does.

For that, it uses playbooks written in YAML. These playbooks describe the automation jobs.

## Install

For Debian based distros, you can use `sudo apt update && sudo apt install ansible -y` to install Ansible on your system.

## Components

Ansible has 6 components:
* Modules
* Module utilities
* Plugins
* Inventory
* Playbooks
* Ansible search path

## Use example

This example supposes you have one Ansible Control Node (where you'll launch the Ansible playbook) and one or more Ansible hosts (to which the control node will connect) that have the control node's public SSH key added to the authorized_hosts file.

The first thing is to tell Ansible who are the hosts to connect to. To do so, open /etc/ansible/hosts and add the following:
```
[example_servers]
server1 ansible_host=10.10.10.2
server2 ansible_host=10.10.10.3

[all:vars]
ansible_playbook_interpreter=/usr/bin/python3
```

In the first section, we specify the IPs of the servers, and in the second we make sure that the Python interpreter that Ansible will use is the version 3.x.

Now, you can run the command `ansible-inventory --list -y` and it should output something like this:
```yaml
Output
all:
  children:
    example_servers:
      hosts:
        server1:
          ansible_host: 10.10.10.2
          ansible_python_interpreter: /usr/bin/python3
        server2:
          ansible_host: 10.10.10.3
          ansible_python_interpreter: /usr/bin/python3
    ungrouped: {}
```

Once that's done, we can test the connection by running the command `ansible all -m ping -u <user>`, in which \<user\> should be substituted by the user that is authorized by the public SSH on the hosts.

If everything goes well, you should get a similar output to this:
```
server1 | SUCCESS => {
    "changed": false, 
    "ping": "pong"
}
server2 | SUCCESS => {
    "changed": false, 
    "ping": "pong"
}
```

And with that, you have Ansible configured and ready to go.

## Ad-hoc commands

Now you can start running ad-hoc commands and playbooks on your servers.

The following is an example of a command that will output the disk usage on each server:
```
ansible all -a "df -h" -u <user>
```

It should give you an output somewhat like this:
```
server1 | CHANGED | rc=0 >>
Filesystem      Size  Used Avail Use% Mounted on
udev            9.9G     0  3.9G   0% /dev
tmpfs           998M  624K  798M   1% /run
/dev/vda1       195G  2.3G  153G   2% /
tmpfs           5.9G     0  3.9G   0% /dev/shm
tmpfs           5.9M     0  5.0M   0% /run/lock
tmpfs           9.9G     0  3.9G   0% /sys/fs/cgroup
/dev/vda15      155M  3.6M  101M   4% /boot/efi
tmpfs           975M     0  798M   0% /run/user/0

server2 | CHANGED | rc=0 >>
Filesystem      Size  Used Avail Use% Mounted on
udev            9.9G     0  3.9G   0% /dev
tmpfs           998M  624K  798M   1% /run
/dev/vda1       195G  2.3G  153G   2% /
tmpfs           5.9G     0  3.9G   0% /dev/shm
tmpfs           5.9M     0  5.0M   0% /run/lock
tmpfs           9.9G     0  3.9G   0% /sys/fs/cgroup
/dev/vda15      155M  3.6M  101M   4% /boot/efi
tmpfs           975M     0  798M   0% /run/user/0
```

An example of a command that will install nmap on all of the servers is `ansible example_servers -m apt -a "name=nmap state=latest" -u <user>`.

As you can see, if you want to execute an `apt` command, you have to use `-m apt` to specify it. 

If you only want to execute a shell command, for example, `free -h` to see the RAM usage on each server, you would use `ansible example_servers -a "free -h" -u <user>`.

## Playbooks

An Ansible playbook is a file that defines a list of task that will be automatically executed by remote computers in an Ansible network.

Here is an example of a playbook that just does a simple `ls -l ~/` on the host machines:
```yaml
---
- name: Runs ls -l ~/ on the specified hosts
  hosts: <servers>
  tasks:
  - name: Runs ls -l ~/, stores the output in registervariable and outputs it
    shell:
      "ls -l ~/"
    register: registervariable
  - debug: msg="{{registervariable.stdout}}"
...
```

You can run it with the command `ansible-playbook -u <user> ansible-playbook.yaml`