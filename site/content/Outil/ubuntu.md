---
title: "Ubuntu"
date: 2018-10-15T10:25:04+02:00
draft: false
tags: ["ubuntu", "installation"]
---

## Installer yum ?

* sudo apt-get install yum

## Install Java

 * `sudo add-apt-repository ppa:webupd8team/java`
 * `sudo apt-get update`
 * `sudo apt-get install oracle-java8-installer`

### Autres solutions
cf https://tecadmin.net/install-java-8-on-centos-rhel-and-fedora/

* Téléchargement
 * `cd /opt`
 * `sudo wget --no-cookies --no-check-certificate --header "Cookie: gpw_e24=http%3A%2F%2Fwww.oracle.com%2F; oraclelicense=accept-securebackup-cookie" "http://download.oracle.com/otn-pub/java/jdk/8u161-b12/2f38c3b165be4555a1fa6e98c45e0808/jdk-8u161-linux-x64.tar.gz" tar xzf jdk-8u161-linux-x64.tar.gz`
 * `sudo tar xzf jdk-8u161-linux-x64.tar.gz`

* Alternatives
 * `cd /opt/jdk1.8.0_161/`
 * `sudo update-alternatives --install /usr/bin/java java /opt/jdk1.8.0_161/bin/java 2`

* Environment

*/etc/environment*
```sh
JAVA_HOME="/opt/jdk1.8.0_161"
JRE_HOME="/opt/jdk1.8.0_161/jre"
PATH="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:$JRE_HOME/bin:$JAVA_HOME/bin"
```

* puis `source environment`


## Security

https://www.digitalocean.com/community/tutorials/how-to-install-nginx-on-ubuntu-16-04
