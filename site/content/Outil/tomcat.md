---
title: "Tomcat"
date: 2018-10-15T10:25:04+02:00
draft: false
tags: ["tomcat"]
---

## Multiples instance

Pour avoir multiples instance d'un tomcat (cf https://dzone.com/articles/run-configure-multiple-instance-in-a-single-tomcat ).

* Télécharger le tomcat classique, et le dézipper
* Le renommer `tcat-base`
* Copier et coller `tcat-base` et le renommer `tcat1` : `cp -R tcat-base tcat1`

### Configurer les instances

Dans le dossier `tcat1` :

* ne garder que les dossier `bin`, `conf`, `logs`, `webapps` et `work`
* supprimer tout ce qu'il y a dans `bin` : `rm -Rf bin/*`
* dans `conf/server.xml`, changer les ports
* eventuellement, ajouter un `setenv.sh` dans le `bin` de `tcat1`.

*tcat1/bin/setenv.sh*
```bash
export CATALINA_OPTS="$CATALINA_OPTS -Xms256m"
export CATALINA_OPTS="$CATALINA_OPTS -Xmx1024m"
export CATALINA_OPTS="$CATALINA_OPTS -XX:MaxPermSize=512m"
```

* eventuellement, pour voir les logs, faire l'étape _Voir les logs_
* Maintenant, copier/coller le dossier `tcat1` autant de fois que nécessaire (ex : `tcat2` et `tcat3`) et changer les ports à chaque fois pour de nouvelles valeurs dans server.xml ET DANS LA CONF LOGS (le fichier logs.xml)

### Configurer la base

Dans le dossier `tcat-base` :

* supprimer les dossiers `conf`, `logs`, `temp`, `webapps`, `work` : `rm -Rf conf logs temp webapps work`
* créer un dossier nommé `controller` : `mkdir controller`
* créer un fichier nommé `startup.sh` dans `controller` : `vim controller\startup.sh`

*tcat-base/controller/startup.sh*
```bash
#!/usr/bin/env sh
app_instance=$1;
BASE_TOMCAT=/location-to-tomcat-parent-directory
export CATALINA_HOME=$BASE_TOMCAT/tomcat
export CATALINA_BASE=$BASE_TOMCAT/$app_instance
$CATALINA_HOME/bin/startup.sh
```

* copier coller `startup.sh` vers `shutdown.sh` : `cp startup.sh shutdown.sh`
* editer `shutdown.sh` pour changer la dernière ligne :

*tcat-base/controller/shutdown.sh*
```bash
#!/usr/bin/env sh
app_instance=$1;
BASE_TOMCAT=/location-to-tomcat-parent-directory
export CATALINA_HOME=$BASE_TOMCAT/tomcat
export CATALINA_BASE=$BASE_TOMCAT/$app_instance
$CATALINA_HOME/bin/shutdown.sh
```

* Ne pas oublier de donner les droits en execution (ex : `chmod u+x controller/*.sh`)

### Démarrer les tomcats

Pour démarrer les différents tomcats, il suffit de spécifier le nom du dossie au script dans controller :

* Pour démarrer `tcat1` : `tcat-base\controller\startup.sh tcat1`
* Pour eteindre `tcat1` : `tcat-base\controller\shutdown.sh tcat1`

## Voir les logs

Pour rendre accessibles les logs sur le net (cf https://stackoverflow.com/questions/18368380/view-tomcat-log-files-in-a-browser )

Cette conf expose les logs de tomcat sur HTTP à travers les services de Tomcat DefaultServer

Dans `MONTOMCAT/conf/Catalina/localhost`, créer le fichier `logs.xml`

*MONTOMCAT/conf/Catalina/localhost/logs.xml*
```xml
<Context override="true" docBase="MONTOMCAT/logs" path="/logs" />
```

ou faire :

```sh
echo '<Context override="true" docBase="MONTOMCAT/logs" path="/logs" />' > MONTOMCAT/conf/Catalina/localhost/logs.xml
```

Puis redémarrer. On peut maintenant voir les logs sur http://localhost:8080/logs/catalina.out

Pour voir tous les logs du dossier, aller éditer `MONTOMCAT/conf/web.xml`, et mettre le paramètre `listings` à `true`.

*MONTOMCAT/conf/web.xml*
```xml
<init-param>
  <param-name>listings</param-name>
  <param-value>true</param-value>
</init-param>
```

Redémarrer, et consulter http://localhost:8080/logs/
