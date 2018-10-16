---
title: "Docker sonar"
date: 2018-10-15T10:25:04+02:00
draft: false
tags: ["docker","sonar","pic"]
---

## Docker Sonar

### Reference

* http://blog.baudson.de/blog/running-a-local-sonarqube-with-docker

### Installation

* `docker pull sonarqube`
* `docker run -d --name sonarqube -p 9000:9000 -p 9092:9092 sonarqube:lastest`
