---
title: "UML"
date: 2018-10-15T10:25:04+02:00
draft: false
tags: ["uml", "documentation"]
---
## PlantUML

### Installation et utilisation simple

* Installer java 
* Créer un dossier dédié : `mkdir c:\perso\uml` puis l'ouvrir `cd c:\perso\uml`
* Récuperer le jar de link:http://sourceforge.net/projects/plantuml/files/plantuml.jar/download[plantUml]  : `curl -L http://sourceforge.net/projects/plantuml/files/plantuml.jar/download > plantuml.jar`
* La commande pour générer une image est : `java -jar plantuml.jar -verbose sequenceDiagram.txt`

### Version poussée : 

* Créer un fichier .bat : 

*generateuml*
```bat
@echo off

if "%~1"##"" GOTO DOC

:GENERATE
java -jar c:\perso\uml\plantuml.jar -verbose %1
goto end

:DOC
echo ECHEC ! il manque un argument. 
echo utilisation : generateuml.bat monficheir.txt
goto end

:END
```

### Utiliser avec VsCode

* Installer l'extension PlantUml pour VsCode : il peut ouvrir la prévisualisation en direct