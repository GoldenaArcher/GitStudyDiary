# GitStudyDiary

I first used GitHub back to 2016 when I was an intern in a small compony. My boss told me that in order to not mess up all the work others 
are doing, we, in particular me, had to use version control tools, such as GitHub. I learned the most basic commands that everyone has to 
know, such as `git pull`, `git push`, `git merge -branch`, `git merge master`, and create **pull request** on the the website. It was a start-up, 
so those commands seemed enough (others are also matured programmers, so the history looked very clean).

However, when I took upper division class in my university, things changed completely. We were asked to use GitHub, which I was comfortable with, 
and felt great. BTW, there were no standards besides we cannot directly commit on the master branch. With Intellij as my IDE, it automatically 
changes a lot of settings that I just added as commits, and pushed commits. Surely, it looked very messy. I did not intend to clean up branch 
history due to the fact git commits was also a part of grading criteria. 

Things changed, of course. Earlier this year when I started my own personal project, I found messed up git histories is really hard to follow, 
and this is the reason I started this diary to learn more about the git commands and Markdown.

# Table of Contents
- [General Standards and Guidelines](#General-Standards-and-Guidelines)
	- [Most General](#Most-General)
	- [Less General](#Less-General)
- [Download](#Download)
- [First Commit](#First-Commit)
- [Git Commands](#Git-Commands)
- [Errors](#Errors)
	- [Rebase Error](#Rebase-Error)

## General Standards and Guidelines<a name="General-Standards-and-Guidelines">

* ___NEVER___ force a push
	
	Resolve merge conflits first
	
* ___NEVER___ rebase a branch that you pushed, or that you pulled from another person

	I learned this lesson, I wanted to rebased the commits that I have already pushed, which resulted `There isn't anything to compare. Nothing to compare, branches are entirely different commit histories` 
	error page on GitHub, thus unable to create PR of my changes.
	
In general, the preferred workflow is:

- create a branch from master, check it out, do your work
- test and commit your changes
- optionally push your branch up to the remote repository (origin) OR
- optionally rebase your branch to master (if your changes are unpublished)
- checkout master, make sure it's up-to-date with upstream changes
- merge your branch into master
- test again (and again)
- push your local copy of master up to the remote repository master (origin/master)
- delete your branch (and remotely, too, if you published it)

From [Git Guidelines and Best Practices](https://wiki.duraspace.org/display/fcrepo/git+guidelines+and+best+practices)

## Download <a name="Download">

- [All git tools](https://git-scm.com/downloads)

	Including all major systems (Windows, Mac OS X, Linux/Unix). I remembered that these links are git bash, and [Git Gui](https://git-scm.com/downloads/guis) 
	is also available. For Mac and Linux, git commands can be directly on the terminal.
		
- IDE support

	Also, the IDEs I use, [Intellij](https://www.jetbrains.com/help/idea/using-git-integration.html) and [Eclipse](https://www.eclipse.org/egit/) has git support as well.
	
- Markdown support

	Eclipse and Intellij can provide plug-in for Markdown. However, sometimes I don't want to open these IDEs just for editing Markdown since it takes 
	a while to start the project inside the IDEs. I also found a great plug-in for Notepad++ on GitHub, [MarkdownViewerPlusPlus](https://github.com/nea/MarkdownViewerPlusPlus).

## First Commit <a name="First-Commit">

After creating a project on the GitHub, if no README.md file is created, GitHub kinds of provide a quick tutorials. I personally found this way has less code. 
Also, there is quite some repeated code here, since `git add` and `git commit` is a ___must___ in changing any file in the workspace. Without commiting, git will 
not allow you to checkout to another branch.

```
# clone the repo at upper-level directory
# if directory is empty, a warning message will appear
$ git clone git@github.com:GoldenaArcher/GitStudyDiary.git
Cloning into 'GitStudyDiary'...
warning: You appear to have cloned an empty repository.
# create README.md file
echo "# GitStudyDiary" >> README.md
git add README.md
git commit -m "first commit"
git push
```

Or follows the guide provided by GitHub with some extra lines. `--allow-unrelated-histories` sometimes is necessary, otherwise it could provoke error message 
`fatal: refusing to merge unrelated histories`.

```
echo "# GitStudyDiary" >> README.md
git init
git add README.md
git commit -m "first commit"
git remote add origin git@github.com:GoldenaArcher/GitStudyDiary.git
git push -u origin master --allow-unrelated-histories
```
	
## Git Commands <a name="Git-Commands">	

### Most General <a name="Most-General">

`git clone git@github.com:GoldenaArcher/GitStudyDiary.git && cd GitStudyDiary`

Clone the desired repo and switch the directory.

`git branch some_branch`

Create a new branch named `some_branch`.

`git checkout some_branch`

I recommend to create a new branch and then use `checkout`. Even though I read that using `checkout` can create a new branch and switch workspace at the same 
time, but I received `error: pathspec 'test' did not match any file(s) known to git.` when I tried to checkout not yet created branch test.

`git add some_file` or `git add .`

The latter one adds all changed file into next commit.

`git commit -m "your message here"`

Add the commit message here to tell others what changes you made in this commit. Also, use `git commit –m “closes #999”` can associate current commit with task 
#999.

`git push`

Push the commit(s) you made to your remote branch. Also works as `git push origin [branch_name]`

`git status`

![git status](https://github.com/GoldenaArcher/GitStudyDiary/blob/master/img/Git_Command/General/Status.PNG)

Display message regarding changed/modified files, and current branch's status.

#### Merge Conflits

Sometimes, when pulling changes from master branch, other people might have already modified some files beforehand. Thus, any changes on already modified file can 
result a merge conflits. Listed code can resolve this problem.

```
# find files with merge conflicts on your local repo
git status
# edit files to resolve the conflicts between
# <<<<<<< HEAD and >>>>>>> BRANCH-NAME
# re build and test
git add .
git commit –m “resolved merge conflict”
git push origin [branchname]
```

### Less General <a name="Less-General">

`gitk`

I am not familar with gitk since I just learned this command. BTW, it seems create a workflow of the current project.

![gitk image](https://github.com/GoldenaArcher/GitStudyDiary/blob/master/img/Git_Command/Less_General/gitk.PNG)

`git config color.ui true`

Display a more colorful message...? I did not see a lot of changes though, perhaps I'm using git bash on Windows, by default the color.ui is true.

### Rebase

#### Rebase Commits
```
# initiate a commit rebase
$ git rebase -i
```

The commit history will display in the command line. All the commands are displayed on the screen. To keep work tree clean, squash and fixup probably 
are most useful commands.

![rebase commit](https://github.com/GoldenaArcher/GitStudyDiary/blob/master/img/Rebase/rebase_commit.PNG)

After `:wq!`, which writes the commands into file, 

Based on the preferred option, changes the command, then do a regular push.

I also used `edit` while doing rebase rather than using `squash` and `fixup`, the estra message at the end of line is `(master|REBASE-i 1/1)`, and the 
messages displayed are:

```
$ git rebase -i
Stopped at eafaf54...  reformat, move 1st commit before git command
You can amend the commit now, with

  git commit --amend

Once you are satisfied with your changes, run

  git rebase --continue

```

Once I use `git commit --amend` to fix my commit message, and use `git rebase --continue` to notify git that I'm done with rebasing, the message 
`(master|REBASE-i 1/1)` is also gone.

## Errors <a name="Errors">
### Rebase Error <a name="Rebase-Error">

#### Method 1, solved the problem, but I do NOT prefer it.

As mentioned, I had this problem, my not preferred way to do that is to checkout to the master branch, and merge my working branch and then push it to the master 
branch. It still needs to create a PR, so I am assuming that I am not directly working on the master branch. Then I deleted my branch, created a new branch. I feel 
there is some other better way to resolve the problem. 

#### Method 2, not tested, but I believe it will work.

I read on the StackOverflow, some people replied that this issues often happened on the forked branches. Since someone might done some changes and it's not on others 
local branch. They suggested to delete the local files and clone it again... Not sure it's best practice, but definitely solve the problem.