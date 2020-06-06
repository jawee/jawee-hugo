---
title: "Run Hugo on server"
date: 2020-06-06T19:31:45+02:00
draft: false
---

To run hugo server while developing websites from the local server, use the following command 

`hugo server -D --bind=<IP> --baseURL=http://<url>:<port>`

For example with the current setup

`hugo server -D --bind=192.168.1.136 --baseURL=http://192.168.1.136:1313`
