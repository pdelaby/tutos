---
title: "Docker sur Unbuntu"
date: 2018-10-15T10:25:04+02:00
draft: false
tags: ["docker","ubuntu"]
---

## Installer sur ubuntu

* `sudo apt update` : update
* `sudo apt install apt-transport-https ca-certificates curl software-properties-common` : installer packages necessaires
* `curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -` : ajout de la clef GPG de docker
* `sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu bionic stable"` : ajout du dépot docker
* `sudo apt update` : re-update
* `apt-cache policy docker-ce` : s'assurer qu'on install docker depuis le repo docker
* `sudo apt install docker-ce` : installer docker
* `sudo systemctl status docker` : check status

## Configurer 
* `sudo usermod -aG docker ${USER}` : s'ajouter sur le user group docker pour eviter de toujour mettre sudo
* `su - ${USER}` : recharger
