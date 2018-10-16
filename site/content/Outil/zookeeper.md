---
title: "Zookeeper"
date: 2018-10-15T10:25:04+02:00
draft: false
tags: ["installation","zookeeper"]
---

## Installation

* Télécharger zookeeeper : `wget http://apache.crihan.fr/dist/zookeeper/stable/zookeeper-3.4.12.tar.gz`
* Dézipper: `tar xzvf zookeeper-3.4.12.tar.gz`
* créer un dossier data : `mkdir zookeeper-3.4.12\data`
* copier la conf : `cp zookeeper-3.4.12\conf\zoo_sample.cfg cp zookeeper-3.4.12\conf\zoo.cfg` 
* editer la conf : `start notepad++ zookeeper-3.4.12\conf\zoo.cfg` et changer le dataDir pour celui qui cient d'être créé
* ajouter `ZOOKEEPER_HOME` comme env et ajouter `%ZOOKEEPER_HOME%\bin` dans le path
* démarrer : `zkserver`
