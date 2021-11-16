# github notes


## to add exiting repo or create new repo 


…or create a new repository on the command line
```
echo "# te" >> README.md
git init
git add README.md
git commit -m "first commit"
git branch -M main
git remote add origin https://github.com/habbis/yourrepo.git
git push -u origin main
```
…or push an existing repository from the command line
```
git remote add origin https://github.com/habbis/yourrepo.git
git branch -M main
git push -u origin main
```
## ssh 

…or create a new repository on the command line
```
echo "# setup_puppet_agent-" >> README.md
git init
git add README.md
git commit -m "first commit"
git branch -M main
git remote add origin git@github.com:habbis/yourrepo.git
git push -u origin main
```
…or push an existing repository from the command line
```
git remote add origin git@github.com:habbis/yourrepo.git
git branch -M main
git push -u origin main
```
