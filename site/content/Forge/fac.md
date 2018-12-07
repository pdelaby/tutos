---
title: "Fac"
date: 2018-11-25T15:37:37+01:00
draft: false
---


* `mkdir -p /home/pdelaby/docker-jenkins/jenkins_home`

* `docker run --name='jenkins' -v /var/run/docker.sock:/var/run/docker.sock -v $(which docker):$(which docker)  -v /home/pdelaby/docker-jenkins/jenkins_home:/var/jenkins_home -p 8999:8080 -p 50000:50000 jenkins/jenkins:latest`

* pdelaby / lemotdepasse



* Installer le plugin "docker", "blueocean", "docker-build-step"