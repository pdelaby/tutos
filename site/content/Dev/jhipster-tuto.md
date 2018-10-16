---
title: "Jhipster"
date: 2018-10-15T10:25:04+02:00
draft: false
tags: ["java","jhipster"]
---


## Ressources

https://developer.okta.com/blog/2017/06/20/develop-microservices-with-jhipster


##  Registry

Il dans /registry

* Front : `yarn`, ça suffit pour builder un coup.
* Back : `mvn spring-boot:run` ;
* Connection :  _http://localhost:8761_
* admin/admin

## Problèmes

### erreur 500 swagger

Si, depuis swagger, quand on essaye d'afficher l'API d'un microservice, il fait une _500 access denied_, vérifier si le cisco de cap' est pas démarré - et l'eteindre.
Sinon, aller dans le registry :

**applycation.yml**
```yaml
  eureka:
      instance:
          appname: jhipster-registry
          instanceId: jhipsterRegistry:${spring.application.instance_id:${random.value}}
          prefer-ip-address: false // <1>
```
<1> changer de `true` à `false`

## Page blanche

Probalement un mvn clean. il faut run `yarn build` et relancer l'appli.


## Gateway

Il est dans /gateway

* Front : `yarn start`, ça suffit pour builder un coup.
* Back : `mvn spring-boot:run` ;


## online shop

On l'utilise a travers la gateway

 * Back : `mvn spring-boot:run` ;

## JDL

Pour importer : `jhipster import-jdl ./my-jdl-file.jdl`

## Developpement

### Script d'initialisation

Avec une base de dev H2 in-memory ou disk, JHipster utilise liquibase :

* initialise les tables à l'aide de changesets dans des fichiers xml
* initialise les données en les chargant à l'aide de fichiers csv.

Il est possible d'utiliser plutôt un fichier SQL pour charger les données. Pour ce faire :

* Changer la configuration de `application-dev.yml`
 * Commenter la ligne `spring.datasource.type : com.zaxer...`
 * Rajouter les lignes
  * `spring.datasource.platform: h2`
  * `spring.datasource.initialize: true`
  * `spring.jpa.properties.hibernate.hbm2ddl.auto: create-drop`

* Le fichier est donc

*application-dev.yml*
```yml
spring:
   # [ ... ]
    datasource:
        #type: com.zaxxer.hikari.HikariDataSource # commenté pour init sql
        url: jdbc:h2:mem:afludialive;DB_CLOSE_DELAY=-1
        username: afludialive
        password:
        platform: h2 #ajouté pour init sql
        initialize: true #ajouté pour init sql
    h2:
        console:
            enabled: false
    jpa:
        database-platform: io.github.jhipster.domain.util.FixedH2Dialect
        database: H2
        show-sql: true
        properties:
            hibernate.hbm2ddl.auto: create-drop #ajouté pour init sql
            hibernate.id.new_generator_mappings: true
            hibernate.cache.use_second_level_cache: true
            hibernate.cache.use_query_cache: false
            hibernate.generate_statistics: true
            hibernate.cache.region.factory_class: io.github.jhipster.config.jcache.NoDefaultJCacheRegionFactory
```

Ensuite, il faut initialiser un fichier `data.sql` dans `src\main\resources`, qui va contenir les données à importer.
Comme les données ne seront pas importées des csv, il faut les mettres dans ce fichier. On peut utiliser https://gist.github.com/hkarakose/cf7f1b5b241dad611ba01c0211f42108[un fichier déjà prêt]

*data.sql*
```sql

-- #### DONNEES DEFAULT JHIPSTER
--USER TABLE

INSERT INTO JHI_USER(ID, LOGIN, PASSWORD_HASH, FIRST_NAME, LAST_NAME, EMAIL, ACTIVATED, LANG_KEY, CREATED_BY, LAST_MODIFIED_BY, CREATED_DATE) VALUES(1, 'system', '$2a$10$mE.qmcV0mFU5NcKh73TZx.z4ueI/.bDWbj0T1BYyqP481kGGarKLG', 'System', 'System', 'system@localhost', true, 'EN', 'system', 'system', SYSDATE);

INSERT INTO JHI_USER(ID, LOGIN, PASSWORD_HASH, FIRST_NAME, LAST_NAME, EMAIL, ACTIVATED, LANG_KEY, CREATED_BY, LAST_MODIFIED_BY, CREATED_DATE) VALUES(2, 'anonymoususer', '$2a$10$j8S5d7Sr7.8VTOYNviDPOeWX8KcYILUVJBsYV83Y5NtECayypx9lO', 'Anonymous', 'User', 'anonymous@localhost', true, 'EN', 'system', 'system', SYSDATE);

INSERT INTO JHI_USER(ID, LOGIN, PASSWORD_HASH, FIRST_NAME, LAST_NAME, EMAIL, ACTIVATED, LANG_KEY, CREATED_BY, LAST_MODIFIED_BY, CREATED_DATE) VALUES(3, 'admin', '$2a$10$gSAhZrxMllrbgj/kkK9UceBPpChGWJA7SYIb1Mqo.n5aNLq1/oRrC', 'Administrator', 'Administrator', 'admin@localhost', true, 'EN', 'system', 'system', SYSDATE);

INSERT INTO JHI_USER(ID, LOGIN, PASSWORD_HASH, FIRST_NAME, LAST_NAME, EMAIL, ACTIVATED, LANG_KEY, CREATED_BY, LAST_MODIFIED_BY, CREATED_DATE) VALUES(4, 'user', '$2a$10$VEjxo0jq2YG9Rbk2HmX9S.k1uZBGYUHdUcid3g/vfiEl7lwWgOH/K', 'User', 'User', 'user@localhost', true, 'EN', 'system', 'system', SYSDATE);

--AUTHORITY TABLE--
INSERT INTO JHI_AUTHORITY(name) VALUES ('ROLE_USER');
INSERT INTO JHI_AUTHORITY(name) VALUES ('ROLE_ADMIN');

--USER_AUTHORITY TABLE--
INSERT INTO JHI_USER_AUTHORITY(USER_ID, AUTHORITY_NAME) VALUES(1, 'ROLE_ADMIN');
INSERT INTO JHI_USER_AUTHORITY(USER_ID, AUTHORITY_NAME) VALUES(2, 'ROLE_USER');
INSERT INTO JHI_USER_AUTHORITY(USER_ID, AUTHORITY_NAME) VALUES(3, 'ROLE_ADMIN');
INSERT INTO JHI_USER_AUTHORITY(USER_ID, AUTHORITY_NAME) VALUES(3, 'ROLE_USER');
INSERT INTO JHI_USER_AUTHORITY(USER_ID, AUTHORITY_NAME) VALUES(4, 'ROLE_USER');
```