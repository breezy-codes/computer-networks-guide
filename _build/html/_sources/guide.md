## to update the book - 
go to the folder prior to the book being `jupyter-books` 
then do `jupyter-book build name/` 

## To push to github pages -
go to the project folder `name`, and main branch 
then do  `ghp-import -n -p -f _build/html` 

## configure the structure of the website in -
_toc.yml with something like this - 

format: jb-book
root: intro
parts:
  - caption: Ciphers
    numbered: false
    chapters:
      - file: ciphers/ceaser-cipher
      - file: ciphers/hill-cipher

## Modify page name and info in - 
_config.yml
such as website name, author and so on
