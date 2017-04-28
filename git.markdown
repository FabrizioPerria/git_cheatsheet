# TERMINOLOGY
## Repository                
>*Remote (or local) location of the project; here the history of our project is stored. It can be seen as the good copy*    
## Working Directory 	
>*Location where we are allowed to make changes, experiments, weirdness; can be seen as the dirty copy*  
## Staging Index  
>*Temporary location where we put our copy when we think it's ready but must be reviewed first*
## Checkout	
>*Bring the content of a Repository into our Working Directory*  
## Add			
>*Bring the content of our Working Directory into the Staging Index*  
## Commit		
>*Bring the content of the Staging Index into the Repository*
## Branch
>*Copy of the project in which we can make operations without affecting the same files in other branches*
## HEAD POINTER
>*Abstract pointer that points to the top of the current branch in the repository (by default is the last committed state). It is going to be the parent of the next commit*
				
# GIT CONFIGURATION
**Configure user global settings (name)**
>*git config --global user.name "my_name"*

**Configure user global settings (mail)**
>*git config --global user.email "user@provider.com"*

**Configure default text editor**
>*git config --global core.editor "vim"*

**Enable colors in git terminal**
>*git config --global color.ui true*

**Set my email address**
>*git config --global sendemail.smtpuser x@y.z*

**SMTP server for gmail**
>*git config --global sendemail.smtpserver smtp.googlemail.com**

**SMTP encryption for gmail**
>*git config --global sendemail.smtpencryption tls**

**SMTP server port for gmail**
>*git config --global sendemail.smtpserverport 587*

N.B.: all the configurations are saved in the **.gitconfig** file in the folder where we configured the user  

# INITIALIZE REPOSITORY
**Ask git to track every change on the project. It creates .git folder**
>*git init*  
N.B.: to stop tracking the project, delete **.git** folder

# THE COMMIT CHAIN
The commit package is similar to a node of a linked list. In this linked list, a node is added in head and so the head points 
to the latest commit done and every node points to the whatever the head was when it got inserted.

# ADD FILES
**Put every change detected (and not ignored) by git into the Staging Index**
>*git add .*

**Put the changes made on file.txt onto the Stanging Index**
>*git add file.txt*

# DELETE FILES
**Delete file from working directory**
>*git rm file*

# RENAME FILES
**Rename file to newFile and put it on the Staging Index**
>*git mv file newFile*

# COMMIT
For the commit operation, git creates a package with changes(aka data), description of the commit and unique checksum(generated starting from data and description) and sends it to the repository.

**Send all the data contained in the staging index to the repository**
>*git commit -m "description"*

**Sign-off the commit**
>*git commit -m "description" -s*

# STATUS AND REPO INFORMATIONS
**Show all the commits done so far**
>*git log*

**Show only the ID and the message for every commit done so far**
>*git log --oneline*

**Show only part of the commit ID**
>*git log --oneline --abbrev-commit*

**Show only the last 3 commits**
>*git log -3*

**Show all the commits done after May 12th 2017**
>*git log --since="2017-05-12"*

**Show all the commits of the last 2 weeks**
>*git log --since="2 weeks ago"*

**Show all the commits done until May 12th 2017**
>*git log --until="2017-05-12"*

**Show all the commits done by Jack**
>*git log --author="Jack"*

**Show the tree with all the branches**
>*git log --oneline --graph --all --decorate*

**Show the checksum of the commit pointed by HEAD in the master branch**
>*cat .git/refs/heads/master*

**Show the status of the repository in terms of:**
* current branch
* uncommitted files (stored in the staging index)
* untracked files (files in the working directory to be added/removed to the staging index)
>*git status*

ROLLBACK AN UNSTAGED FILE (UNDO-like)
git checkout -- file		replace the messed up file of the wroking directory with the last committed version of that file

UNSTAGE FILES
used if we send a file to the staging area and we modify it before commit:
git add file
echo "change" >> file

now we can:
git add file				overwrites file in the staging area
OR
git reset HEAD file			unstage

MODIFY A COMMITTED FILE WITHOUT A NEW COMMIT
possible only for the very last commit!!! Don't use it if possible....
git commit --amend "description"

REVERT TO AN OLD COMMIT
Receive a copy of an old version of the branch (is not going to delete the newer ones!is just a copy)
git checkout id_of_the_commit				checkout the whole commit
git checkout id_of_the_commit -- file		checkout only the file specified at that point in time
git checkout -b my_branch					create and switch to my_branch

DIFF
git feature to check differences between 2 versions of a file

git diff file

if we stage new/file first....git add file
git diff --staged is going to tell us which changes have been made to file

RESET
There are 3 types of reset in git:
	git reset --soft id_commit 		moves the HEAD to the specified commit(undo a commit)
	git reset --mixed id_commit		moves the HEAD and the staging index to whatever they were in the specified commit(undo a staging
	git reset --hard id_commit		completely reverts the commit (DANGEROUS...undo a change in the files)


BRANCHES
git branch				show all the branches created(* is on the current branch)
git branch my_branch	create a branch called my_branch

CREATE A PATCH
git format-patch original_branch..new_branch
git format-patch -o patches/ -3		create a patch for each one of the last 3 commits and save 
									them in patches folder

git format-patch --subject-prefix="prefix]["  add prefix in the subject of the email-format patch

SEND EMAIL
script/get_maintainer.pl my_patch.patch

git send-email --to x@y.z --cc a@b.c --chain-reply-to --in-reply-to=last_message_id patches/*

GITIGNORE
.gitignore is a file that we can create and where we specify (through regex) what git should ignore in the folder. For instance,
if we write some code and we compile it, we may want to keep track of the source files only an not the executable or the object
files. In .gitignore we can write
*.o
*.out
*.exe
