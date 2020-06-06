---
title: "Running node applications until stopped"
date: 2020-06-06T14:49:14+02:00
draft: false
description: "How to keep node applications running in the background, and restarting after system reboot"
---

### Using PM2
You can use PM2, it's a production process manager for Node.js applications with a built-in load balancer.

###### Install PM2

`$ npm install pm2 -g`


###### Start an application
`$ pm2 start app.js`

###### If you using express then you can start your app like

`pm2 start ./bin/www --name="app"`

###### Listing all running processes:

`$ pm2 list`

It will list all process. You can then stop / restart your service by using ID or Name of the app with following command.

`$ pm2 stop all`
`$ pm2 stop 0`                
`$ pm2 restart all`

###### To display logs

`$ pm2 logs ['all'|app_name|app_id]`

