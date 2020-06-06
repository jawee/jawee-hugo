---
title: "Jenkins Hugo setup"
date: 2020-06-06T20:03:20+02:00
draft: false
---

Setup for making it possible to run jenkins in a docker-container, with support for building Hugo websites.
###### Dockerfile
```Dockerfile
FROM jenkins/jenkins

USER root
RUN apt-get update && apt-get install -y --no-install-recommends apt-utils
RUN apt-get install -y rsync
RUN wget https://github.com/gohugoio/hugo/releases/download/v0.72.0/hugo_0.72.0_Linux-64bit.deb
RUN dpkg -i hugo_0.72.0_Linux-64bit.deb
RUN rm hugo_0.72.0_Linux-64bit.deb

USER jenkins
```

###### Docker-compose
```yml
---
version: "2"
services:
  jenkins:
        build:
          context: jenkins/
        #image: jenkins
        container_name: jenkins
        environment:
            - PUID=1000
            - PGID=1000
            - TZ=Europe/Stockholm
        volumes:
            - /home/figge/containers/jenkins/jenkins_home:/var/jenkins_home
        ports:
            - 8081:8080
            - 50000:50000
        restart: unless-stopped
```


