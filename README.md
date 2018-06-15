# McGill SI 2018

Tutorials for microbial genomics and genomic epidemiology.

## Deployment

The tutorials have been deployed here: https://sepsis-omics.github.io/mcgill-si-2018/

## How to work on them locally

### Install the mkdocs tools
```
% pip install mkdocs markdown-include mkdocs-extensions mkdocs-material
```

### Clone the repo
```
% git clone https://github.com/sepsis-omics/mcgill-si-2018.git
% cd mcgill-si-2018
```

### Browse the site locally without deploying to public internet
```
% mkdocs serve
```
Open your web browser to http://127.0.0.1:8000/ and leave it open.
This will update automatically as you make changes to the documenation.

### The master document is a YAML file
```
% less mkdocs.yml
```

### The actual Markdown pages are in the `docs` folder:
```
% ls docs
index.md about.md modules/ 

# modules/lessons
% cd modules
% ls

dna/ prot/ rna/ met/  

# the content for a module
% cd dna
% ls dna
index.md images/
```

To add a new page, say a page on the 'Minia' genome assembler, find the
right location and create a page.  In this case it would be
`docs/modules/denovo/minia.md`.  Write the tutorial in that file, and then add
the file to the master document `mkdocs.yml` in the correct section.

### When you are happy, add it the repo
```
git add docs/modules/denovo/minia.md
git commit -m "Added minia" mkdocs.yml docs/modules/denovo/minia.md
git push
```
Your private local web version http://127.0.0.1:8000/ will also auto-update.

### To deploy the whole lot to the *public* website
```
mkdocs gh-deploy --clean --message "Added minia"
```
This first builds a web HTML version of our Markdown hierarchy into the `site/` folder, then pushes it to a special
branch of the github repo called `gh-pages` which GitHub makes available at the public URL
https://sepsis-omics.github.io/mcgill-si-2018/

## Authors

* Anna Syme
* Torsten Seemann
* Simon Gladman
* Dieter Bulach
* Anders Goncalves da Silva
* Lauren Cowley

