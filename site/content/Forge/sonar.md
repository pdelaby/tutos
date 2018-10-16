---
title: "Sonar"
date: 2018-10-15T10:25:04+02:00
draft: false
tags: ["sonar", "pic"]
---


## Docker

###  Reference
* http://blog.baudson.de/blog/running-a-local-sonarqube-with-docker

## Installation Local

* seul : `docker run -d --name sonarqube -p 9000:9000 -p 9092:9092 sonarqube`

* `docker pull sonarqube`
* `docker pull postgres`
* On lance postgres on lui donnant une database et un user sonar `docker run --name sonar-postgres -e POSTGRES_USER=sonar -e POSTGRES_PASSWORD=secret -d postgres`
* On lance sonarqube et on lui demande de se connecer   `docker run -d --name sonarqube --link sonar-postgres:pgsonar -p 9000:9000 -e SONARQUBE_JDBC_USERNAME=sonar -e SONARQUBE_JDBC_PASSWORD=secret -e SONARQUBE_JDBC_URL=jdbc:postgresql://pgsonar:5432/sonar sonarqube:latest`

## Installation Centos

### PostGresql

* Installer postgres : `sudo yum install postgresql-server`
* Init the cluster : `sudo postgresql-setup initdb`
* Accept md5 pwd : `sudo vim /var/lib/pgsql/data/pg_hba.conf`  : trouver les lignes à la fin et remplacer les 2 _indent_ par _md5_ et aussi le _peer_ par _trust_
* Démarrer et activer
** `sudo systemctl start postgresql`
** `sudo systemctl enable postgresql`
* Changer le mot de passe : `sudo passwd postgres`
* Se connecter `su - postgres`
* Créer un utilisateur `createuser sonar`
* Passer au shell `psql`
* Mettre un password pour sonar : `ALTER USER sonar WITH ENCRYPTED password 'motdepasseFort';`
* Créer une nouvelle base : `CREATE DATABASE sonar OWNER sonar;
* Quitter : `\q`

### Sonar

#### Fichiers
* Récupere : `wget https://sonarsource.bintray.com/Distribution/sonarqube/sonarqube-6.7.1.zip`
* Unzip : `sudo unzip sonarqube-6.7.1.zip -d /opt`
* Renommer : `sudo mv /opt/sonarqube-6.7.1 /opt/sonarqube`

#### Configuration
* Editer la conf : `sudo nano /opt/sonarqube/conf/sonar.properties`
 * Décommenter et editer
```
sonar.jdbc.username=sonar
sonar.jdbc.password=StrongPassword
```

 * Décommenter `#sonar.jdbc.url=jdbc:postgresql://localhost/sonar`
 * Décommenter et éditer `sonar.web.context=` pour `sonar.web.context=/sonar`

#### Administration

* Créer le groupe sonar : `groupadd sonar`
* Créer l'user sonar : `useradd -c "Sonar System User" -d /opt/sonarqube -g sonar -s /bin/bash sonar`
* Chmod : `chown -R sonar:sonar /opt/sonarqube`
* OU Editer  ``/opt/sonarqube/bin/sonar.sh` : trouver RUN_AS_USER decommenter et y mettre l'username : `RUN_AS_USER=sonar` OU creer le fichier suivant
* OU Créer un fichier pour la gestion : `sudo nano /etc/systemd/system/sonar.service`

*sonar.service*
```
[Unit]
Description=SonarQube service
After=syslog.target network.target

[Service]
Type=forking

ExecStart=/opt/sonarqube/bin/linux-x86-64/sonar.sh start
ExecStop=/opt/sonarqube/bin/linux-x86-64/sonar.sh stop

User=sonar
Group=sonar
Restart=always

[Install]
WantedBy=multi-user.target
```

#### Apache

{{% notice warning %}}
A vérifier, ne fonctionne probablement pas
{{% /notice %}}

*sonar.conf*
```
ProxyPass         /sonar  http://localhost:9000/sonar nocanon
ProxyPassReverse  /sonar  http://localhost:9000/sonar
ProxyRequests     Off
AllowEncodedSlashes NoDecode

# Local reverse proxy authorization override
# Most unix distribution deny proxy by default (ie /etc/apache2/mods-enabled/proxy.conf in Ubuntu)
<Proxy http://localhost:9000/sonar*>
  Order deny,allow
  Allow from all
</Proxy>
```

* Puis démarrer : `sudo systemctl start sonar`


## Scanner

Commande simple : `mvn sonar:sonar -Dsonar.host.url=http://localhost:9000 -Dsonar.login=73bLOGIN643855`

### Scanner classique
* téléchargement : https://sonarsource.bintray.com/Distribution/sonar-scanner-cli/sonar-scanner-cli-3.0.3.778-windows.zip
* extraire dans `C:\devtools\sonar-scanner`
* ajouter une var d'env : `SONAR_SCANNER=C:\devtools\sonar-scanner`
* ajouter au path : `%SONAR_SCANNER%\bin`
* tester en cmd : `sonnar-scanner -h`

### Scanner windows
* cf : https://docs.sonarqube.org/display/SCAN/Scanning+on+Windows
* installer aussi visual studio et le framework .net https://www.microsoft.com/fr-fr/download/details.aspx?id=53344


## Analyse

### Jenkins

* Dans jenkins, global configuration,
 * checker _Enable injection of SonarQube server configuration as build environment variables_

### Vrac

`sonar-scanner.bat -Dsonar.projectKey=projectkey -Dsonar.sources=. -Dsonar.host.url=http://localhost:9000 -Dsonar.login=laclefgeneree`

### Python

* tuto : https://docs.sonarqube.org/display/SCAN/Analyzing+with+SonarQube+Scanner#AnalyzingwithSonarQubeScanner-Installation

### Angular project

On peut simplement faire `sonar-scanner.bat -Dsonar.projectKey=projectkey -Dsonar.language=ts -Dsonar.sources=src/app/ -Dsonar.host.url=http://localhost:9000 -Dsonar.login=laclefgeneree`

Ou `sonar-scanner.bat -Dsonar.projectKey=projectkey -Dsonar.language=ts -Dsonar.sources=src/app/ -Dsonar.host.url=http://localhost:9000 -Dsonar.login=laclefgeneree`

## Installer le runner sur centos

* installer sur centos : `wget https://sonarsource.bintray.com/Distribution/sonar-scanner-cli/sonar-scanner-cli-3.0.3.778-linux.zip`
* `sudo unzip sonar-scanner-cli-3.0.3.778-linux.zip -d /opt/sonarqube/`
* `sudo mv /opt/sonarqube/sonar-scanner-3.0.3.778-linux/ /opt/sonarqube/sonar-scanner`
*  `sudo ln -s /opt/sonarqube/sonar-scanner/bin/sonar-scanner /opt/sonar-scanner`
* ou l'ajouter dans `/etc/profile.d/global_aliases.sh` ( `alias sonar-scanner='/opt/sonarqube/sonar-scanner/bin/sonar-scanner'`)
