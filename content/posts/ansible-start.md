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
   hosts: proxmox
   tasks:
     - name: Set authorized key took from file
       authorized_key:
         user: figge
         state: present
          key: "{{ lookup('file', '/home/figge/.ssh/id_rsa.pub') }}"
```

To run it on new servers
`ansible-playbook playbook.yml -e "ansible_user=<user> ansible_ssh_pass=<userpassword>"`

To run it on servers where key is already added (pointless)
`ansible-playbook playbook.yml`

