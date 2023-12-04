**GIT I**

#### Setting up GIT credentials
We can configure git with our user information from the terminal by running a set of git commands:

```bash 
git config --global user.name Peterbajhogh

git config --global user.email 201904997@post.au.dk

git config --global credential.helper store

git config --list
```
#### Practical Advice on the use of GIT
1. Use branches to work on new features or bug fixes
2. Make regular commits – not one large commit in the end
3. Make the commit messages meaningful so collaborators can understand what has been done
4. Do a pull before a push – this ensures you have the latest code local
5. If you have a conflict, resolve it as quickly as possible
6. As ”Pull requests” are not inherently a Git feature, you can work on a separate branch and, when done, then go
   to Github and make a Pull Request
