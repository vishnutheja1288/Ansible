# Ansible
Ansible-Playbooks
@techbeatly
@DevOpsHint
$ ansible all -m ping

$ ansible <host group>[index value] -m ping

checking any files on the client side
$ ansible all -a "ls"

$ ansible all -a "touch file1"

copy the files across the remote nodes

$ ansilbe all -m copy -a "src=./samplefile dest=/tmp/samplefile"

system information

$ ansible <hostgroup> -m setup

$ ansible <hostgroup> -m setup -a "filter=ansible_os_family"

$ ansible <hostgroup> -m setup -a "filter=ansible_memory_mb"

$ ansible <hostgroup> -m shell -a "top -c -b| head -15"

$ ansilbe <hostgroup> -m shell -a "ps -eo pid,ppid,%mem,%cpu,cmd,--sort=-%mem | head"

-------------------------------
INVENTORY: list of host of group of hosts : /etc/ansbile/hosts
MODULES: programs that perform the actual work of the task of a play. These modules was used under task session in our playbook
TASKS: set of instructions which can be performed by using modules so in the task we useually called modules


install apache in unbuntu

---
- name: setup an apache webserver
  hosts: webservers
  become: true
  tasks:
    - name: install apache2
      apt: name=apache2 update_cache=yes state=latest
