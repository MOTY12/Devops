
# Introduction 

Git is a distribution version control system. it solve the problem of sharing source code efficiently and keeping track of changes made to source code

## Initializing a git Repository

Firstly, Before initializing a git repo, Install a git on your system then follow the below steps.

- Open your terminal
- on your terminal create a working folder or directory using this command `mkdir <projectName>`
- change your directory to the working folder `cd <projectName>`
- Intialize a new repo in you local using this command `git init`

![git init](images/Screenshot%202023-10-27%20at%2008.17.59.png)

## Making your first commit 
**git commit** is saving the changes you made to your files. when you make a commit, git takes the snapshot of the current state of yout repo and save a copy in .git folder in your working directory. Below is the steps of making a git commit.

- create a new file in your working directory `touch index.txt`

- write any thing inside the file 

- Add your changes to git staging area using this command `git add .`

- To commit the changes to git run the command `git commit -m "initial commit"`.

![git init](images/Screenshot%202023-10-27%20at%2016.59.00.png)

## Working with branches
**git branch** helps to create a different copy of your source code. It is used for new features developed and untested which is yet to be added to the live application.

### Create first branch **git branch**
- Create a new branch using this command: `git checkout -b <new branch>`. **-b  flag helps to create and change into new branch**

- To list the branches on local git repo use this command `git branch`

- change into an old branche `git checkout <branch-name>`

- Merge a branch into another branch ` git merge <branch_name>`
- Deleting a git branch  `  git branch -d <branch_name>`
- To learn more about the branches in git use the command: `git branch -help`

![git init](images/Screenshot%202023-10-29%20at%2022.52.02.png)


## Pushing your local git to remote github repo

Create a github account if you don't have any and a repo. [Click here](https://docs.github.com/en/get-started/onboarding/getting-started-with-your-github-account) to learn more . We push our source code to the remote repo by going through this steps.

- Add a remote repository to the local repo `git remote add origin <link to your github repo>`. To get the remote link just click on the green button, then copy the link.
- After commiting your changes on the local repo, Push the code to the remote repo `git push origin <branch name>` 

## Clone remote git Repo
It is used to pull the rmote repo to the local using this commsnd `git clone <link to your remote repository>`

## Mark down syntax  
It is a a lightweight markup language that is widely used for formatting plain text. It is used for documentation in git which commonly used in creating documents, READme files, forum post and web pages. Below are commonly used syntax
# Heading 1

## Heading 2

### Heading 3

_italic_ or _italic_
**bold** or **bold**

- Item 1
- Item 2
- Item 3

1. First item
2. Second item
3. Third item

Links: This command below is used for links 
``[visit darey.io](https://www.darey.io)``

Images: This command below is used for display image 
``![Alt Text](https://example.com/image.jpg)``

`console.log('Welcome to darey.io')`


Note: https://learn.microsoft.com/en-us/contribute/content/markdown-reference