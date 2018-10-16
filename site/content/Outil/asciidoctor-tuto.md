---
title: "AsciiDoctor"
date: 2018-10-15T11:52:04+02:00
draft: false
tags: ["documentation","asciidoc"]
---

Cette documentation reprends et condense les indications données sur le site officiel https://asciidoctor.org

## Installation

* Installer _Ruby_ en utilisant https://rubyinstaller.org/downloads/
* Installer _Asciidoctor_ : `gem install asciidoctor`


## Générer la documentation

* Cloner le dépôt Git contenant la documentation ( ex : `git clone https://github.com/pdelaby/tutos.git`)
* Se rendre dans le dossier : `cd tutos`
* Générer les fichiers HTML : `asciidoctor *.adoc`
* Ouvrir les fichiers HTML : `start chrome asciidoctor-tuto.html`

## Editer la documentation

* Commencer un document, par exemple en utilisant https://github.com/pdelaby/tutos/blob/master/template/doc.adoc

**.doc.adoc**
```
include::template/doc.adoc[]
```

* Il suffit ensuite de générer le HTML avec la commande `asciidoctor doc.adoc`

### LiveReload

Il existe de nombreuses méthodes pour éditer un document et avoir un aperçu direct ( cf https://asciidoctor.org/docs/editing-asciidoc-with-live-preview/ ).
L'extension Atom _LiveReload_ est pratique mais ralentis considérablement Atom.

La solution recommandé est donc d'installer _Guard_ comme décrit dans l'installation.

* Installer _Guard_ et _Guard-Shell_ : `gem install guard guard-shell`
* Installer _Atom_  (https://atom.io/) et le package _language Asciidoc_ ( ou n'importe quel éditeur de texte avec la coloration syntaxique Asciidoc)
* Dans le répertoire où sont présents les fichiers asciidoc, créer un fichier Guard.
Ce fichier va permettre de lancer un Guard qui va surveiller les changements sur le fichier _adoc_. A chaque enregistrement, il va re-générer le html.

**.Guardfile**
```ruby
include::template/Guardfile[]
```

* Puis, en console se rendre dans le dossier et lancer `guard start`
* Comme le guard occupe la console, pour plus de simplicité, faire
 * `asciidoctor monfichier.adoc` pour générer un première fois
 * `start chrome monfichier.html` pour ouvrir le fichier
 * `guard start` pour commencer à surveiller
* Pour rafraichir automatiquement la page dans Chrome, installer une extension du type link:https://chrome.google.com/webstore/detail/livereload/jnihajbhpnppcggbcgedagnkighmdlei[LiveReload]
* Une fois installé, il suffit de lui demander de recharger la page automatiquement

## Tips & Tricks

## Code

Pour le code, il faut  :

* installer _coderay_ : `gem install coderay`
* l'activer en haut des fichiers : `:source-highlighter: coderay`

### Les icones

IMPORTANT: Les icônes passent pas ?

Pour que les icons fonctionnent, il faut que `:icons: font` soit à la première ligne.
De plus, avec _guard_, ils ne marchent pas...


### Générer du word

* Installer Pandoc : `csudo choco install pandoc -y`
* Convertir un doc : `pandoc -s asciidoctor-tuto.html -o output2.docx`
* Donc la commande : `asciidoctor %1.adoc | pandoc -s %1.html -o %1.docx`
