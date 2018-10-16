---
title: "Angular"
date: 2018-10-15T11:52:04+02:00
draft: false
---

### Npm install

Quand `npm install` fonctionne pas a cause du composant _Visual C++ "VCBuild.exe"_,
il suffit de faire :

* `npm install --global --production windows-build-tools`
* `npm update`


### Desactiver la sécurité chrome

Chrome dev : `--disable-features=CrossSiteDocumentBlockingIfIsolating`

### Ng

Si ng ne fonctionne pas ou n'est pas trouvé : `npm install -g @angular/cli`

### Jenkins

Sur la plateforme il faut d'abord installer l'environnement

 * Ajouter le dépot `sudo yum install epel-release`
 * Télécharger `curl --silent --location https://rpm.nodesource.com/setup_9.x | sudo bash -`
 * Installer `sudo yum install -y nodejs` et `sudo yum install -y gcc-c++ make`
 * Installer bzip2 sinon rien ne marche : `sudo yum install bzip2`

Configurer npm ?

* npmrc vim `/usr/etc/npmrc`
* y ajouter `phantomjs_cdnurl=https://bitbucket.org/ariya/phantomjs/downloads`

### PrimeNg avec Angular et JHipster

cf https://stackoverflow.com/questions/44162427/steps-to-integrate-primeng-with-jhipster

* Installer les dépendances avec yarn :
** `yarn add primeng --save`
** `yarn add @angular/animations --save`
* Ajouter _primeNg_ dans vendor.css

**.src/main/app/content/scss/vendor.scss**
```css
// primeng
@import 'node_modules/primeng/resources/primeng.min';
@import 'node_modules/primeng/resources/themes/bootstrap/theme';
```

* Ajouter dans _app.module.ts_ 
 * les imports; exemple `import { AccordionModule, RatingModule, CalendarModule } from 'primeng/primeng';`
 * les éléments dans `imports:[]`

* en cas de problème avec les images, `cp -R .\node_modules\primeng\resources\images\* .\src\main\webapp\content\scss\`

### Python

En cas de problème yarn install avec python : 

* `npm --add-python-to-path='true' --debug install --global windows-build-tools`
* `npm install --global --production windows-build-tools`
