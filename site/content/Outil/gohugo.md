---
title: "GoHugo"
date: 2018-10-15T11:52:04+02:00
draft: false
tags: ["documentation","gohugo"]
---

## Installation

Avec choco : `csudo choco install hugo -y`

Utilisation de VsCode recommandée

## Initialisation

* `hugo new site quickstart`
* `cd quickstart` 
* `git init .`
* Thème
 * `git submodule add https://github.com/matcornic/hugo-theme-learn.git themes/learn`
 * `git submodule add https://github.com/pdelaby/hugo-theme-learn.git themes/learn-pdy` (learn avec des tags, en cours)
 * `echo 'theme = "learn"' >> config.toml`

{{% notice warning %}}
Attention, si il y a un submodule, il faut spécifier `--recurse-submodules` lors du clônage du dépot : `gi t clone --recurse-submodules https//monurl`
{{% /notice %}}


## Demarrage 
* `hugo server -D`

## Theme 

* https://github.com/matcornic/hugo-theme-learn.git

## Syntaxe

### Note

```
{{%/* notice note */%}}
A notice disclaimer
{{%/* /notice */%}}
```

renders as

{{% notice note %}}
A notice disclaimer
{{% /notice %}}

### Info

```
{{%/* notice info */%}}
An information disclaimer
{{%/* /notice */%}}
```

renders as

{{% notice info %}}
An information disclaimer
{{% /notice %}}

### Tip

```
{{%/* notice tip */%}}
A tip disclaimer
{{%/* /notice */%}}
```

renders as

{{% notice tip %}}
A tip disclaimer
{{% /notice %}}

### Warning

```
{{%/* notice warning */%}}
An warning disclaimer
{{%/* /notice */%}}
```

renders as

{{% notice warning %}}
A warning disclaimer
{{% /notice %}}

## Ajouter les tags

Dans le theme learn, pour ajouter des tags : 

1. Modifier le single.html  : `themes/hugo-theme-learn/layouts/_default/single.html`

En dessous de la ligne des headers, rajouter le partial tags : 

```html
{{ partial "header.html" . }}
{{- partial "tags.html" . -}}
```

2. Créer le fichier tags : `themes/hugo-theme-learn/layouts/partials/tags.html`

```html
<p> Tags : </p>
<ul class="pa0">
  {{ range .Params.tags }}
   <li class="list">
     <a href="{{ "/tags/" | relLangURL }}{{ . | urlize }}" class="link f5 grow no-underline br-pill ba ph3 pv2 mb2 dib black sans-serif">
       {{- . -}}
     </a>
   </li>
  {{ end }}
</ul>
```

3. Editer le fichiers lists : `themes/hugo-theme-learn/layouts/_default/list.html` et rajouter en dessous de content

```html
<ul>
{{ range .Pages }}
	<li><a href="{{.URL}}">{{.Title}}</a></li>
{{ end }}
</ul>
```