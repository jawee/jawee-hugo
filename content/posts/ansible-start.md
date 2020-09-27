---
title: "Ansible thingies"
date: 2020-09-27T14:31:45+02:00
draft: false
---

* Group: Example
* * Inventory file: a file with server lists and groups
* * User: the user that logs onto the server
*
* ##### For ssh key auth
* `ansible -i <inventory file> <group> -m <command:ping> -u <user> (if not same as user on machine)`
*
* ##### For password auth:
* `ansible -i <inventory file> <group> -m <command:ping> -u <user> --ask-pass`
*
* `ansible -i <inventory file> <group> -m <command:ping> -u <user> -K`
*
* ##### Run free -h
* `ansible <group> -a "free -h"`
*
*
* If you don't add a module (-m), it will automatically add `-m commands` to the task.
*
*
