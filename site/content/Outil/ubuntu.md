---
title: "Ubuntu"
date: 2018-10-15T10:25:04+02:00
draft: false
tags: ["ubuntu", "installation"]
---

## Installer yum ?

* sudo apt-get install yum

## Install Java

cf https://tecadmin.net/install-java-8-on-centos-rhel-and-fedora/

* Téléchargement
** `cd /opt`
** `sudo wget --no-cookies --no-check-certificate --header "Cookie: gpw_e24=http%3A%2F%2Fwww.oracle.com%2F; oraclelicense=accept-securebackup-cookie" "http://download.oracle.com/otn-pub/java/jdk/8u161-b12/2f38c3b165be4555a1fa6e98c45e0808/jdk-8u161-linux-x64.tar.gz" tar xzf jdk-8u161-linux-x64.tar.gz`
** `sudo tar xzf jdk-8u161-linux-x64.tar.gz`
* OU 
** `sudo add-apt-repository ppa:webupd8team/java`
** `sudo apt-get update`
** `sudo apt-get install oracle-java8-installer`



* Alternatives
** `cd /opt/jdk1.8.0_161/`
** `sudo update-alternatives --install /usr/bin/java java /opt/jdk1.8.0_161/bin/java 2`
* Environment

*/etc/environment*
```sh
JAVA_HOME="/opt/jdk1.8.0_161"
JRE_HOME="/opt/jdk1.8.0_161/jre"
PATH="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:$JRE_HOME/bin:$JAVA_HOME/bin"
```

* puis `source environment`


## Jenkins

* suivre : https://www.digitalocean.com/community/tutorials/how-to-install-jenkins-on-ubuntu-16-04
* `wget -q -O - https://pkg.jenkins.io/debian/jenkins-ci.org.key | sudo apt-key add -`
* `echo deb https://pkg.jenkins.io/debian-stable binary/ | sudo tee /etc/apt/sources.list.d/jenkins.list`
* `sudo apt-get update`
* `sudo apt-get install jenkins`
 * Si il refuse pour une histoire de *deaemon* , ajouter les package universe : `sudo add-apt-repository universe`  
 * Si jenkins ne build pas sur docker : https://stackoverflow.com/questions/20430371/my-docker-container-has-no-internet
 * Si il veux pas cloner TFS ou probleme de cipher
  * aller editer le /etc/ssh/ssh_config
* ajouter `KexAlgorithms diffie-hellman-group1-sha1,curve25519-sha256@libssh.org,ecdh-sha2-nistp256,ecdh-sha2-nistp384,ecdh-sha2-nistp521,diffie-hellman-group-exchange-sha256,diffie-hellman-group14-sha1`
  * et `Ciphers aes256-cbc,aes192-cbc,aes128-cbc`


* Pour le rajouter avec docker `sudo usermod -a -G docker jenkins`

* Attention a démarrer jenkins en classique, si il est demarré en sudo ça va foutre la merde durant le sh


## Security

https://www.digitalocean.com/community/tutorials/how-to-install-nginx-on-ubuntu-16-04
