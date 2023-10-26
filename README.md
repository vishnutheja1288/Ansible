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


$ansible-playbook playbook.yaml --check

$ ansible-playbook palybook.yaml

$ ansible-palybook -i <inventoryfile> playbook.yaml


Ansible variable declare:
	1) playbook with vars
	2) create new file for variable
	3) Declare variable in the inventory file

---
- name: playbook with vars
  hosts: webservers
  vars:
    title: Welcome to Ansible
  tasks:
    - name: Ansilbe playbook variable declartion example
      debug:
        msg: "{{ title }}"

vars.yaml
package_name = apache
palybook.yaml
---
- name: setup apacheweb server
  hosts: webservers
  become: true
  tasks:
    - include_vars: vars.yaml
    - name: install apache2
      apt: name={{ package_name }} update_cache=yes state=latest


ANSIBLE LOOPS
--------------

ansible offers the loop and with _<lookup> keyword execute a task multiple times.

simple creation users
--------------------

---
- name: create users using loop
  status: true
  hosts: webservers
  tasks:
    - name: add users
      ansible.builtin.users: #user:
        name: "{{item}}"
        state: present
      loop:
        - test1
        - test2
        - test3

$ ansible all -m shell -a "cat /etc/passwd | tail -10 "

Iteration over a list of hashes
-------------------------------

---
- name: iterating over a list of hashes
  hosts: webservers
  become: true
  tasks:
    - name: add users
      ansible.builtin.users:
        name: "{{ item.name }}"
        state: present
        groups: "{{ item.groups }}"
      loop: 
        - { name: 'testuser3', groups: 'root'}
        - { name: 'testuser4', groups: 'root'}

$ ansible-playbook <playbook.yaml>
$ ansible all -m shell -a "tail -10 /etc/passwd "

iterating over a dictionary
-------------------------------

---
- name: iterating over a dictionary
  hosts: webservers
  become: true
  tasks:
    - name: using dict2items
      debug:
        msg: 
        - "the user {{item.key}} is {{item.value}}"
      loop: "{{ user_types | dict2items }}"
      vars:
        user_types:
          Environment: dev
          Payment: Application



loops with indexes
-------------------

---
- name: indexes in ansible
  hosts: webservers
  become: true
  tasks:
    - name: indexes with ansible loops
      debug:
        msg: "the car at {{item.0}} is {{item.1}}"
      with_indexed_items:
        - "Toyota"
        - "BMW"
        - "Hundai"
        - "Kia"
        - "Lexus"
        - "Aura"
        - "Benz"
        - "Tata"
        - "Volvo"
