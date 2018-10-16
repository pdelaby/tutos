---
title: "Jenkins"
date: 2018-10-15T10:25:04+02:00
draft: false
tags: ["jenkins","pic"]
---

## Installer Jenkinsfile

* Suivre : https://www.vultr.com/docs/how-to-install-jenkins-on-centos-7

## Tomcat
### Installer et configurer
* télécharger tomcat :  `wget http://wwwftp.ciril.fr/pub/apache/tomcat/tomcat-8/v8.5.24/bin/apache-tomcat-8.5.24.tar.gz`
* dézipper tomcat
* editer `vim conf/tomcat-users.xml` et ajouter

*tomcat-users.xml*
```xml
<role rolename="manager-script"/>
<role rolename="manager-gui"/>
<user username="admin" password="password" roles="manager-gui"/>
<user username="deploy" password="password" roles="manager-script"/>
```

* editer `vim conf/server.xml` et changer les ports

### Deployer et supprimer en commande

Pour deployer :

`wget --http-user=xxx --http-password=xxx 'http://localhost:8081/manager/text/deploy?war=file:/var/lib/jenkins/workspace/demo-rest-back/target/demo-rest-back.war&path=/demo-rest-back' -O -`

Pour supprimer :

`wget --http-user=xxx --http-password=xxx 'http://localhost:8081/manager/text/undeploy?path=/demo-rest-back' -O -`

{{% notice note %}}
Dans une pipeline jenkins, il est nécessaire de supprimer avant de deployer sinon ça échouera
{{% /notice %}}


{{% notice note %}}
Le chemin du war doit être en absolu
{{% /notice %}}


## Jenkins

### Pipeline: lire une propriété

* installer le plugin _Pipeline Utility Steps_ qui va permettre de read le file
* lire la propriété dans la pipeline

**WARNING**: Pour récuperer la propriété (def ...) il faut bien utiliser les guillements

*Jenkinsfile*
```groovy
pipeline {
    agent any

  environment {
    def props = readProperties  file:'/var/lib/jenkins/jobconf/tomcat.properties'
    def deployUrl= "${props['tomcat.deploy.url']}"
  }
    stages {
        stage('prepare'){
            steps{
                echo "${deployUrl}"
            }
        }

    }

}
```

### Pipeline : utiliser des credentials
* Utiliser la gestion des credentials pour ajouter par ex des credentials tomcatdeploy
* la pipeline

*Jenkinsfile*
```groovy
pipeline {
    agent any
    stages {
        stage('prepare'){
            steps{
                withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: 'tomcatdeploy', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD']]) {
                        sh 'echo uname=$USERNAME pwd=$PASSWORD'
                }
            }
        }
    }
}
```


### Deployer un rapport
Pour déployer un rapport, utiliser une step après les stages

.Jenkinsfile
```groovy
post{
     always{
         // enregistre les rapports de test
         junit 'target/surefire-reports/TEST-*.xml'

         // enregistre les rapports JSON pour le build
         cucumber '**/*.json'

         // enregistre les rapports HTML
         publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, keepAll: false, reportDir: 'target/cucumber', reportFiles: 'index.html', reportName: 'HTML Report', reportTitles: 'Rapport de tests cucumbers'])

         // publish la doc
         publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, keepAll: false, reportDir: 'target/generated-docs', reportFiles: 'demo-rest-back.html', reportName: 'Doc', reportTitles: 'documentation'])

         // publish la javadocgit
         publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, keepAll: false, reportDir: 'target/site/apidocs', reportFiles: 'index.html', reportName: 'JavaDoc', reportTitles: 'JavaDoc'])
     }
 }
```

Attention, par défaut les JS et CSS ne sont pas pris en compte dans les rapports HTML. Il faut autoriser Jenkins à le faire :

* depuis la console jenkins, en executant `System.setProperty("hudson.model.DirectoryBrowserSupport.CSP", "")`
* en modifiant le fichier jenkins :
 * `sudo vim /etc/sysconfig/jenkins`
 * modifier `JENKINS_JAVA_OPTIONS="-Djava.awt.headless=true '-Dhudson.model.DirectoryBrowserSupport.CSP= ' "``

Ici, une propriété pointe vers le path de tomcat, et les répertoires sont créés dans _webapps_.
De plus l'utilisateur jenkins à des droits pour publier sur tomcat (autorisation d'ecriture)


## Docker Jenkins

### References :
* https://www.youtube.com/watch?v=6BIry0cepz4[30 Jenkins features and plugins you wished you had known about before! by Joep Weijers]

### Jenkins :
* `docker run -p 8080 -p 50000:50000 jenkinsci/jenkins:latest`
* plugins  TODO automate
* manage > Configure Global Security >  authorization : role -based strategy

### Plugins
. Installer les plugins lors du premier usage
. Lister les plugins installés
