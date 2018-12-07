---
title: "Consul"
date: 2018-11-16T11:02:10+01:00
draft: false
tags: ["infra","sma","archi"]
---

## Démarrage

Lancer consul `docker run -d --name=dev-consul -e CONSUL_BIND_INTERFACE=eth0 -p 8500:8500 consul`

Ou avec le fichier

```yml
version: '3.5'
services:
  consul-dev:
     image: consul
     #le nom du container est important il sera utilisĂ© pour s'y connecter (depuis notre appli main)
     container_name: consul-dev
     environment:
        - CONSUL_BIND_INTERFACE=eth0
     ports:
        - 8500:8500
```

