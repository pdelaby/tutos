---
title: "Kafka"
date: 2018-10-15T10:25:04+02:00
draft: false
tags: ["kafka", "installation"]
---

## Installation

Kafka nécessite [zookeeper](../zookeeper)

* Kafka
 * Télécharger : https://kafka.apache.org/downloads dans 
 * Dézipper: `tar xzvf kafka_2.12-1.1.0.tgz`
 * Ouvrir le dossier `cd kafka_2.12-1.1.0`
 * Créer le dossier logs: `mkdir logs`
 * Editer la conf : `start notepad++ config/server.properties`
 * Changer logs.dir vers le dossier nouvellement créé
 * démarrer kafka : `.\bin\windows\kafka-server-start.bat .\config\server.properties`

* kafka-manager : http://edbaker.weebly.com/blog/install-and-evaluation-of-yahoos-kafka-manager
* démarrer sur 8091

* Landoop - relou
 * `git clone https://github.com/Landoop/kafka-topics-ui.git`
 * `cd kafka-topics-ui`
 * `npm install -g bower`
 * `npm install -g http-server`
 * `npm install`
 * `bower install`
 * `http-server -p 8080 .`

