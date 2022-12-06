---
title: Réalisation du rapport avec Pandoc
author:
- Renan Declercq
- Florian Etrillard
- Felix Pereira
- Groupe E
---

# Réalisation du rapport avec `Pandoc`

*Procédure réalisée sur Windows avec WSL Ubuntu.*

## Préparation

### Téléchargement des packages nécessaires

```bash
sudo apt install pandoc pandoc-sidenote
```

[Pandoc-sidenote](https://github.com/jez/pandoc-sidenote) est un filtre `Pandoc` (un petit script) que l'on peut utiliser pour faire apparaître les notes de bas de page à droite de la page (et non en bas), ce qui améliore et facilite beaucoup la lecture.

### Mise en place de la structure de fichiers nécessaires

Création d'un répertoire de sortie qui contiendra un dossier pour les images et un dossier pour les styles.

```bash
mkdir output && cd output
mkdir images styles
```

Téléchargement de la template HTML utilisée pour générer le rapport, ainsi que les feuilles de styles `CSS` pour le rendre plus agréable à lire. [^pandoc-templates-and-styles]

```bash
# Template HTML
wget https://raw.githubusercontent.com/jez/pandoc-markdown-css-theme/master/template.html5

# Styles CSS
cd styles
wget https://raw.githubusercontent.com/jez/pandoc-markdown-css-theme/master/public/css/{skylighting-paper-theme,theme}.css

# Copie "de sécurité" des styles (pour faire des modifications en gardant les originaux)
cp skylighting-paper-theme code-highlighting.css
cp theme.css style.css
cd ../
```

[^pandoc-templates-and-styles]: Afin de ne pas tout coder à la main, nous avons utilisé la *template HTML* et les *styles CSS* fournis par [ce projet](https://github.com/jez/pandoc-markdown-css-theme/).

Copie des images de chaque rapport dans le dossier d'images du rapport final.

```bash
cp ../rapport\ étape\ {1..4}/images/* images/
```

## Génération du rapport avec Pandoc

### Commande utilisée

```bash
pandoc \
--from markdown \
--to html5 \
--template=customtemplate \
--css="./styles/style.css" \
--css="./styles/code-highlighting.css" \
--filter pandoc-sidenote \
--toc \
--number-sections \
--metadata title="SAE 203 - Réseaux" \
--metadata subtitle="Installation d'un service réseau" \
--output "./rapport final.html" ../introduction/introduction.md ../*/rapport\ étape\ {1..4}.md ../pandoc/pandoc.md ../conclusion/conclusion.md
```

### Explication des options utilisées [^pandoc-manual]

- `from` : pour spécifier le format d'origine des fichiers à convertir
- `to` : pour spécifier le format de destination des fichiers convertis
- `template` : permet de spécifier le template utilisé pour la conversion [^template]
- `css` : ajouter un fichier `CSS` au fichier `HTML` généré (*ils sont ajoutés grâce au template HTML*)
- `filter` : permet d'utiliser un filtre lors de la conversion (`pandoc-sidenote`) [^filters]
- `toc` : générer la table des matières
- `number-sections` : permet de numéroter les sections/titres de manière automatique
- `metadata` : permet de spécifier certaines métadonnées utilisés dans le template `HTML`
- `output` : pour spécifier le fichier de sortie
- `...` : liste des fichiers `markdown` à convertir (l'intro, les 4 rapports, la génération du rapport final, la conclusion).

[^pandoc-manual]: [Pandoc manual](https://pandoc.org/MANUAL.html)
[^filters]: Cela ressemble beaucoup au fonctionnement des *pipes* sur `Linux`.
[^template]: Fichier contenant des instructions (boucles, conditions, variables...) remplacés par des données réelles (du texte par exemple) à l'étape de la conversion.

## Script final

```bash
# A ne pas refaire à chaque fois
sudo apt install pandoc pandoc-sidenote

mkdir output && cd output
mkdir images styles

wget https://raw.githubusercontent.com/jez/pandoc-markdown-css-theme/master/template.html5
cd styles
wget https://raw.githubusercontent.com/jez/pandoc-markdown-css-theme/master/public/css/{skylighting-paper-theme,theme}.css
cd ../
cp ../rapport\ étape\ {1..4}/images/* images/

# Génération du rapport
pandoc \
--from markdown \
--to html5 \
--template=customtemplate \
--css="./styles/style.css" \
--css="./styles/code-highlighting.css" \
--filter pandoc-sidenote \
--toc \
--number-sections \
--metadata title="SAE 203 - Réseaux" \
--metadata subtitle="Installation d'un service réseau" \
--output "./rapport final.html" ../introduction/introduction.md ../*/rapport\ étape\ {1..4}.md ../pandoc/pandoc.md ../conclusion/conclusion.md
```
