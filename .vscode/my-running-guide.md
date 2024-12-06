# to update the book - 

cd /home/breezy/Documents/GitHub/Jupyter-Books

jupyter-book build computer-networks-guide

or rebuild with

jupyter-book build --all computer-networks-guide

# to push the book to GitHub Pages -

cd /home/breezy/Documents/GitHub/Jupyter-Books/computer-networks-guide

make sure in main - 

ghp-import -n -p -f _build/html
