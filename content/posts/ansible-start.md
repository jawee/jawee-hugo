---
title: "Ansible thingies"
date: 2020-09-27T14:31:45+02:00
draft: false
---

Group: Example
 - Inventory file: a file with server lists and groups
 - User: the user that logs onto the server

##### For ssh key auth
`ansible -i <inventory file> <group> -m <command:ping> -u <user> (if not same as user on machine)`

##### For password auth:
`ansible -i <inventory file> <group> -m <command:ping> -u <user> --ask-pass`

`ansible -i <inventory file> <group> -m <command:ping> -u <user> -K`

##### Run free -h
`ansible <group> -a "free -h"`


If you don't add a module (-m), it will automatically add `-m commands` to the task.


##### Playbook for adding ssh key to all servers
```yaml
---
- name: Set authorized key took from file
   hosts: all
   tasks:
     - name: Set authorized key took from file
       authorized_key:
         user: <user>
         state: present
          key: "{{ lookup('file', '/home/<user>/.ssh/id_rsa.pub') }}"
```

To run it on new servers
`ansible-playbook playbook.yml -e "ansible_user=<user> ansible_ssh_pass=<userpassword>"`

To run it on servers where key is already added (pointless)
`ansible-playbook playbook.yml`

To run it on a specific group
`ansible-playbook playbook.yml -l <group>`

##### Playbook for running update
```yaml
---
- name: Update server
  hosts: all
  become: yes

  tasks:
    - name: Run apt upgrade
      apt:
        upgrade: yes
         update_cache: yes
    - name: run dist-upgrade
      apt:
        upgrade: dist
```

To run sudo command
`ansible-playbook playbook.yml -e "ansible_sudo_pass=<password>"`
