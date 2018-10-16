---
title: "Elasticsearch"
date: 2018-10-15T10:25:04+02:00
draft: false
tags: ["elasticsearch", "installation"]
---


## Outils
Réalisé avec cywgin et chocolatey (pour installer wget, notepad++, gunzip et unzip)

## Install

From https://www.elastic.co/start


* ElasticSearch
 * Télécharger :  `wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-6.1.1.zip`
 * unzip : `unzip elasticsearch-6.1.1.zip`
* Kibana
 * Télécharger : `wget https://artifacts.elastic.co/downloads/kibana/kibana-6.1.1-windows-x86_64.zip`
 * unzip  : `unzip kibana-6.1.1-windows-x86_64.zip` (c'est un peu long, ya tout node_module à sortirn)
*  XPACK
 * Aller dans elasticsearch `cd elasticsearch-6.1.1`
 * OPTIONNEL : Installer xpack `elasticsearch-6.1.1\bin\elasticsearch-plugin install x-pack`
* Démarrer EX
 * start elastic `elasticsearch-6.1.1\bin\elasticsearch`
 * OPTIONNEL : generate passwords `elasticsearch-6.1.1\bin\x-pack\setup-passwords auto`
 * (du coup noter les password, et il faudra les configurer dans kibana et filebeat plus tard)
* OPTIONELL : si password, configurer kibana fichier pour y renseigner les infos elasticsearch
 * `start notepad++ kibana-6.1.1-windows-x86_64\config\kibana.yml`
 * décommenter _elasticsearch.username_ et _elasticsearch.password_ et renseigner les infos
* Démarrer kibana
 * `kibana-6.1.1-windows-x86_64\bin\kibana`
* Cerebro (anciennement kopf)
 * Récupération  `wget https://github.com/lmenezes/cerebro/releases/download/v0.7.2/cerebro-0.7.2.zip`
 * unzip : `unzip cerebro-0.7.2.zip`
 * run : `cerebro-0.7.2\bin\cerebro.bat`
 * se connecter
* Installer filebeat
 * dl : `wget https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-6.1.2-windows-x86_64.zip`
 * extraire : `csudo unzip filebeat-6.1.2-windows-x86_64.zip -d "C:/Program Files"`
 * renommer : `csudo mv "c:\Program Files\filebeat-6.1.2-windows-x86_64" "c:\Program Files\Filebeat"`
* Installer filebeat la suite
 * Démarrer un powershell en admin
 * ouvrir filebeat : `cd 'C:\Program Files\Filebeat\'`
 * lancer l'install: `.\install-service-filebeat.ps1`
* Configurer Filebeat
 * Créer le dossier `C:\ProgramData\elasticsearch\logs`
 * Récupération des données `wget https://download.elastic.co/demos/kibana/gettingstarted/logs.jsonl.gz`
 * Dézipper, et copier le fichier dans le dossier logs qui vient d'être créé (  le fichier `logs.jsonl` )
 * Configurer filebeat : `start notepad++ C:\Program Files\Filebeat\filebeat.yml`
 * editer le premier protractor de type log pour le mettre à enabled true et lui configurer le path (et les passwords si besoin)

*filebeat.yml*
```yaml
- type: log

# Change to true to enable this prospector configuration.
enabled: true

# Paths that should be crawled and fetched. Glob based paths.
paths:
 # - /var/log/*.log
  - C:\programdata\elasticsearch\logs\*
```

 * démarrer filebeat : (en powershell) : `Start-Service filebeat` 
 * on peut suivre les logs dans `C:\ProgramData\filebeat\logs`

Filebeat balance tout dans elasticsearch. Il faudrait juste configurer un mappging ( se réferer à https://www.elastic.co/guide/en/kibana/current/tutorial-load-dataset.html ) et ce serait réglé.
