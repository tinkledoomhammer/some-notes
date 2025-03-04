
[https://classroom.udacity.com/courses/ud123]

# Version Control with Git

## Setting up bash
After installing, add to .bash_profile:
```
# Enable tab completion
source ~/.udacity-terminal-config/git-completion.bash

# Change command prompt
source ~/.udacity-terminal-config/git-prompt.sh

# colors!
red="\[\033[38;5;203m\]"
green="\[\033[38;05;38m\]"
blue="\[\033[0;34m\]"
reset="\[\033[0m\]"

export GIT_PS1_SHOWDIRTYSTATE=1

# '\u' adds the name of the current user to the prompt
# '\$(__git_ps1)' adds git-related stuff
# '\W' adds the name of the current directory
export PS1="$red\u$green\$(__git_ps1)$blue \W
$ $reset"
```

Then create .udacity-terminal-config/ git-completion.bash and git-prompt.sh.

Both files are quite large and will not be included verbatim.

Finally, set up git user info:
```
# sets up Git with your name
git config --global user.name "<Your-Full-Name>"

# sets up Git with your email
git config --global user.email "<your-email-address>"

# makes sure that Git output is colored
git config --global color.ui auto

# displays the original state in a conflict
git config --global merge.conflictstyle diff3

git config --list
```

and optionally, a code editor
```
git config --global core.editor "code --wait"
```

## First commands
Required Linux commands: ls,mkdir,cd,rm

### git init
Creates all the fiels needed by the repo in the .git directory.
#### .git directory contents

* Config file - saves repo-specific settings
* Description file -only used by the GitWeb program, will be ignored in this class.
* hooks directory - where client- or server-side scripts that can hook into git lifecycle events
* info directory - contains the global exclude file
* objects directory - where all the commits are stored
* refs directory - holds pointers to commits (branches or tags or whatevah
Shouldn't really mess with any of them except the hooks directory.

### git clone
```
git clone <source-url-or-folder> [<local-folder-name>]
```
### git status
Gives basic info about the staging area.

### git log
Default info: checksum, author, date, commit message
Navigation
| Action |One Line | half page | whole page| 
| - | - | - | - |
|Down| j or $\downarrow$ |d|f|
|Up| k or $\uparrow$  | u | b |
|Quit |q|

```git log --oneline```
: shows 7 digit sha and commit message only

```git log --stat```
: Lists files changed with the # of lines +/-

```git log --patch```
: ```git log -p```
Shows the changes, line-by-line

```git log -p -w```
: excludes changes to whitespace

```git log -p <checksum>```
: starts listing from the specified commit

```git log --decorate```
: includes tag info

```git log --oneline --decorate --graph --all```
: shows all the branches on the repo 


### git show

No arguments:

Shows the most recent commit, and the info is the same as git log -p.

```git show <checksum```
: Shows only the specified commit

Other switches

* --stat the same as git log --stat, removes patch details
* -p or --patch - the default, can be used with --stat
* -w ignores changes to whitespace

## Working with Commits

### git commit

Staging area->repo.

```git commit [-m "commit message"]```
: Makes a commit. If the message is omitted, then the default editor will open. Save the file and exit to make the commit.

The message should be <60ish characters and explain what the commit does, not how.  If an explanation of why is necessary, then put it after the main description, with a blank line between the main message and the explanation:
```
Incerase footer height

The footer was not displaying correctly on devices between
570-630 pixels.  This change addresses a known bug on Android KitKat
```

```git commit --amend```
: Lets you edit the most recent commit

* works just like git commit but it changes the most recent commit instead of making a new one
* add files you forgot to stage
* change the commit message



### git add
Adds files from the working directory to staging area
```git add <filename> [<filename>] ...```
: Add files to the staging area.

```git add .```
: Adds all files in the working directory to the staging area, including files in subfolders.

```git rm --cached <file>``` 
: to remove a file from the staging area.

### git diff

With no arguments, it shows the diff(s) between the working directory and the last commit.This command is used internally by ```git log -p```. it displays

* The files that have been modified.
* The location of the lines that have been added/removed.
* The actual changes that have been made.


The [Official Docs for git diff](https://git-scm.com/docs/git-diff).

### .gitignore
Lists files that git should ignore. (i.e. so that ```git add .``` works without adding junk fiels. Case sensitive except for file extensions (?).

Globbing

* blank lines can be used for spacing
* ```#``` marks a line as a comment.
* ```*``` matches 0 or more characters
* ```?``` matches exactly one character
* ```[abc]``` matches a or b or c
* ```**``` matches nested directories. ```a/**/z``` matches
	* a/z
	* a/b/z
	* a/b/c/z


## Tags, branches, merging

### git tag

Used to lable specific commits.

```git tag -a tagname``` creates an annotated tag named tagname.
: without the -a option, it creates a 'lightweight' tag which doesn't include:

	* the person who made the tag
	* the date the tag was made
	* a message for the tag
	* perhaps more(?)

```git tag```
: Lists all tags in the current repo.

```git tag -d tagname```
: Deletes the tag

```git tag -a tagname checksum```
: Tags a specific commit

### git branch
By default the first branch name is master.
when a commit is added to a repo, it moves the branch pointer.
The 'head' pointer points to the active branch.

```git branch```
: lists the branch names in the repo
Puts an * next to the active branch

```git branch <branchname>```
: creates a new branch with the specified name

```git branch -d <branchname>```
: deletes branch that has been merged

* won't work on the current branch
* won't work on branches with un-merged commits
* the ```-D``` option forces deletion



### git checkout
```git checkout <branchname>```
: switches the active branch

* moves the head pointer to the selected branch
* removes all files and directories that git is tracking in the working directory
* pulls from the repository all the files and directories from the commit that the branch points to.

```git checkout -b <new branchname>```
: creates a new branch and checks it out.


### git merge

* fast-forward merges basically just consolodate branches where only one has changes
* merging moves the current branch

```git merge <branch to merge>```
: What merge does

* Find the most recent commit in the history of the current and target branch
* Include all changes from that point to both branches
* makes a new commit on the active (head) branch with all the changes from the other branch


Dealing with Conflicts

* After the merge command, if there are conflicts, there will be a notification in the merge command output and git status will have a 'unmerged paths:' section.
* The files will have both sets of conflicting lines
* VSCode will nicely highlight them and create clickable links to help resolve the conflict
* Instructions from ```git status```:
	* fix conflicts and run "git commit"
	* use "git merge --abort" to abort the merge
* Contents of conflicted files:
	* ```<<<<<<< HEAD``` The beginning of the conflict section
	* ```||||||| merged common ancestors``` everything below this line (until the next indicator) shows you what the original lines were
	* ```=======``` is the end of the original lines, everything that follows (until the next indicator) is what's on the branch that's being merged in
	* ```>>>>>>> <merge-in-branch>``` is the ending indicator of what's on the branch that's being merged in
* To resolve:
	* choose which lines to keep
	* delete everything else
	* don't forget to remove the indicator lines.
	* ```git add``` the conflict files
	* commit

## Undoing changes

* ```git commit --amend```
	* i.e. for types in commit message
	* don't use it on publicly available commits -- it will cause confusion for others
* ```git revert <commit checksum>```
	* changes made in the specified commit are reversed
	* creates a new commit
* ```git reset <targetcommit>```
	* deletes a commit
	* POTENTIALLY DANGEROUS
	* ```git reset --mixed HEAD~``` undoes one commit, puts the changes made in the most recent commit in the working directory.
	* ```git reset --hard HEAD~``` deletes all of the changes in the most recent commit.
	* ```git reset --soft HEAD~``` moves the changes from the most recent commit to the staging area.
	* for safety see:
		* [git-reflog](https://git-scm.com/docs/git-reflog)
		* [Rewriting History](https://www.atlassian.com/git/tutorials/rewriting-history)
		* [reflog, your safety net](http://gitready.com/intermediate/2009/02/09/reflog-your-safety-net.html)
		* `git rebase`
		



### Relative commit references

* `^` indicates the parent commit
* `~` indicates the *first* parent commit
	* The *first* parent of a merged commit is the one that was merged into.  The other is the one that was merged into the *first* parent.
* `^^` indicates the grandparent commit
* `^2` is the second parent commit
* `~2` is the first parent of the first parent
* `~3^2` is the second parent of the first great-grand-parent.


### Submodules
* Adding a submodule to an existing project
	`git submodule add https://gitbub.com/asdf/repo`
	* It will by default clone into a new folder with the name of the repo
* .gitmodules - a file that lists submodules with their path and url
* git will not track changes to submodules while in the parent module
	* It will view it as a folder and a commit
	* 






















> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE3NzU4MzE5MTksLTIyNzU1MTU4XX0=
-->