---
title: "Docker PIC"
date: 2018-10-15T11:52:04+02:00
draft: false
tags: ["docker","jenkins"]
---

## Installer Jenkins sur docker

En suivant le tutoriel https://ifritltd.com/2017/07/06/dockerize-jenkins-2-setup-with-sonarqube-declarative-build-pipeline/[Dockerizing Jenkins 2, Part 1: Declarative Build Pipeline With SonarQube Analysis]

* Récuperer l'image : `docker pull jenkins/jenkins:lts`
* Récuperer la liste des plugins souhaités. Sur un jenkins existant, dans la console

```groovy
Jenkins.instance.pluginManager.plugins.each{
  plugin ->
    println ("${plugin.getShortName()}:${plugin.getVersion()}")
}
```

* Les mettre dans plugins.txt
* Créer le dockerfile

**.Dockerfile**
```
#this is the base image we use to create our image from
FROM jenkins/jenkins:lts

#just info about who created this
MAINTAINER Kayan Azimov (email)

#get rid of admin password setup
ENV JAVA_OPTS="-Djenkins.install.runSetupWizard=false"

#automatically installing all plugins
COPY plugins.txt /usr/share/jenkins/ref/plugins.txt
RUN tr -d '\r' < /usr/share/jenkins/ref/plugins.txt | /usr/local/bin/install-plugins.sh
```

* Builder l'image : `docker build -t myjenkins .`

* Ensuite faire les downloads :
 * `mkdir downloads`
 * `cd downloads`
 * `curl -O http://apache.mirror.anlx.net/maven/maven-3/3.5.2/binaries/apache-maven-3.5.2-bin.tar.gz`
 * `curl -O http://ftp.osuosl.org/pub/funtoo/distfiles/oracle-java/jdk-8u144-linux-x64.tar.gz`

* Sur windows 10, monter des volumes foire. Il faut donc aller sur le répertoire à monter et faire
 * Clic droit > propriétés > Security > modify > add > lamachine\docker-user (on peut chercher) et lui donner les droits

* Démarrer docker en montant le volume :
```
docker run -p 8080:8080 -v ${PWD}:/var/jenkins_home/downloads --rm --name myjenkins myjenkins:latest
```
