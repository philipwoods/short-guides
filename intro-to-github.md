# Introduction

This guide is a collection of the very basics of how to use Git and Github in your
work. If you find that you need additional information beyond this document, the
[Github documentation](https://docs.github.com/get-started) is quite good. Github
also created a very helpful
[Git cheat sheet](https://training.github.com/downloads/github-git-cheat-sheet/)
for quick reference.

Note that this guide describes how to use Git and Github to track and back up
your own work. If all you want is to download someone's existing code from Github,
you can simply use the command `git clone <url>"` where the url of the desired
repository can be found in the green "Code" dropdown menu on the repository's
Github website.

Because of its common usage in scientific computing, all example commands provided
in this guide assume a UNIX-based operating system such as Linux or MacOS.

If you have questions about any of this information and the documentation above
isn't clear, feel free to ask me (Philip) for help.

# The basics

## What is Git?

Git is a version control system which allows you to retain not just the final version
of your project, but all intermediate versions along the way. This capability is like
non-destructive editing, where each change you make can be reverted, but Git also
provides some additional functionality.

Git accomplishes this by storing information about your project at commit points which
you identify in a repository. At each commit point, Git stores a list of changes made
to the project, the author of those changes, and a message describing the contents of
the changes. These commit points are linked together to create a (possibly branching)
history of the project.

## What is Github?

Github is a website which you can use to remotely store your Git repositories. It is
possible to use Git without using Github. However, Github is a very useful resource,
and I strongly recommend you use it with your projects. Some of the benefits Github
can provide include:

- Remote backup in case of local data loss
- Publishing or sharing projects publicly
- Working collaboratively on a project with others

While Github makes projects publicly visible by default, it is possible to create a
limited number of private repositories which will be stored through Github but not
made publicly available.

# Initial setup of Git/Github

## Installing and configuring Git

To start using Git, first check whether it is installed on your computer. If you
use WSL, Git is probably already installed. Otherwise, look
[here](https://git-scm.com/downloads) to download Git for your system. There is
a GUI-based Git client available for download, but this guide will focus on the
command line interface due to its more general applicability (for example, the
GUI may not be usable when working on remote servers like ocean).

Once Git is installed, there are a few initial setup steps.

1. Set a name to be used to mark you as the author of your commits. This doesn't
need to be the same as your Github username. In your terminal,
run `git config --global user.name "<name>"`
2. Set an email to attach to your git commits. This should be the same
email connected to your Github account. In your terminal, run
`git config --global user.email "<email-address>"`
3. To enable helpful color in git output, run `git config --global color.ui auto`
4. (Optional) Choose a text editor for crafting commit messages. If you
have a preferred text editor, run `git config --global core.editor=<editor>`
in your terminal. By default, git will use nano.

## Setting up secure connections with Github

If you want to work with remote repositories, you will first need to set up a
Github account and verify the email associated with it. Once you have done that,
you will need to set up credentials so that Github can verify your identity when
communicating with you over the command line. This process will look slightly
different depending on whether you use HTTPS or SSH as your secure channel with
Github, but the end result will be essentially the same either way. You will need
to remember which authentication protocol you chose, because the URL referring to
a Github repository is slightly different when using HTTPS or SSH.

### Connecting with HTTPS

I haven't used HTTPS very much to communicate with Github, so take this with a
grain of salt. My understanding is that you simply use your normal Github account
credentials to authenticate this way, but Github may be removing this in favor of
personal access tokens. These personal access tokens are more secure in that you
can choose the priveleges associated with each token, but this may not be necessary
for typical users. For more information about setting up personal access tokens,
see the guide [here](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens).

### Connecting with SSH

Using SSH to authenticate requires the use of a public and private cryptographic
key. The public key is provided to Github and the private key is kept on your
local device. If you want to connect to Github from multiple devices (e.g. your
work laptop and ocean) you should create a new key pair on each device. To create
these key pairs, run the following command in your terminal, substituting the
email address you provided to Git in the previous step:

        `ssh-keygen -t ed25519 -C "<github-email>"`

If you are prompted to "enter a file in which to save the key," just press ENTER
to accept the default location and name. If you are prompted to enter a passphrase,
either press ENTER to choose no passphrase, or enter a password which will be
required each time you initiate an SSH connection as an extra layer of security.
This passphrase does not need to be your Github account password.

After the files have been created, you need to provide the public key to Github.
After logging into your account, go to `Settings>SSH and GPG keys` and click the
green "New SSH key" button. Add a title for the key (e.g. the name of the device
the key is on). In your terminal enter the command `cat ~/.ssh/id_ed25519.pub`
and copy the output into the "Key" field on Github, then click "Add SSH key" to
finish the process.

# Setting up a project repository

In your Github account, click the ["New repository?"] button [near the upper right corner].
Choose a relevant name for the project, scroll down, and click the [button] to create
the repository. Github should display a page with two different sets of commands to
run in your terminal. If you have already created a Git repository with `git init`
on your local machine and simply need to link it to Github, you should run the second
set of commands. Otherwise, run the first set of commands.

In order to start working in the repository, you need to tell Git which files to pay
attention to. In your new repository, type `git status` into your terminal. This will
display a message describing "staged" files, "unstaged" files, and/or "untracked"
files. If a file shows up in the "untracked" list, that means Git recognized that it
is present in the project directory, but Git is not tracking its history. To tell Git
to track a file, enter `git add <filename>` into your terminal. If you run `git status`
again, you should see that file has moved from the list of "untracked" files to the
list of "staged" files. We will cover what this means in the next section.

There may be files that will be stored in your project directory that you never want
Git to track. By default, any untracked file will always appear in the "untracked"
section of a `git status` message, which can add a lot of clutter. If you have files
you want Git to ignore forever, you can create a file named `.gitignore` in your
project directory and list the names of those files in it, one file name per line.
You can also use bash text expansion syntax to specify multiple files at once:
adding the line `*.pdf` to your `.gitignore` file will tell git to ignore all PDFs
in that directory. If you change your mind later, you can simply remove the name of
a file from the `.gitignore` file and add it to Git as described in the previous
paragraph.

# Storing commits in Git and Github

As mentioned before, Git tracks changes to files by creating a chain of commits, each
of which is a snapshot of the history of the files in the repository at a particular
time. When you change the contents of a file that Git is tracking, it can detect
those changes. When you want to record your changes, creating a new snapshot of the
history of the file(s), you will need to create a commit. As a general rule of thumb,
it's a good idea to commit often rather than waiting until many unrelated changes
have accumulated.

## Making commits to local repositories

Once you have reached a point that you want to store as a commit, save your work and
enter `git status` in your terminal. Git will display a list of all modified files,
describing them as "unstaged." You need to tell Git which file(s) should be included
in the new commit by "staging" them. You can do this by entering `git add <filename>`
in your terminal. If you want to stage all of the changed files at once, you can
enter `git add -a` meaning "add all." If you re-enter `git status`, you should
see that your file(s) are now described as "staged" or listed under "Changes to be
committed." When you create a commit, only the changes which have been staged will
be recorded, even if there are other changes to files in the project. Note that if
you continue to edit a file after staging it, those later edits are not staged.
This is represented in the output of `git status` as the file appearing in both the
"unstaged" and "staged" sections. To stage these later edits, simply `git add` the
file again.

You may want to see what changes have been made to your files before choosing what
changes should be committed. You can do this by entering `git diff` into your
terminal. This will also show you how Git tracks changes to files. Git views files
line by line, and if you change a line it records that as removing the old version
of the line and adding the new version of the line to the file. Added lines are
prefixed with `+` and removed lines are prefixed by `-` and may be highlighted with
color depending on your terminal settings. As a quick reference, `git diff` will
show all *unstaged* changes compared to the last commit, `git diff --cached` will
show all *staged* changes compared to the last commit, and `git diff HEAD` will
show all changes (staged or unstaged) compared to the last commit.

Once you have staged all of the relevant changes, it is time to actually record
the commit. At its most basic, this can be done by entering `git commit` in your
terminal. At this point, Git will open a text editor and prompt you to write a
commit message describing the contents of the commit (i.e. what changes have
been made). These messages can have a title and a body which are separated by a
blank line. Common practice is to keep the title brief (less than 50 characters)
and if more detailed explanation of the commit is required, to put that in the
body of the message. Once you save the commit message, Git will record the commit.
If your commit message doesn't need a body, you can also enter it directly using
the `-m` flag, like `git commit -m "<message>"`.

## Interacting with remote repositories

When you initially set up a repository, Github tells you to designate a "remote"
called `origin` which is associated with the Github URL of your repository. This
is how Git knows where to look when communicating with the Github remote backup
of your repository. The standard idiom is that this remote repository is always
given the shorthand name `origin`.

The main way you will interact with a project's remote repository is sending
information about commits to be stored in the remote. This is accomplished with
the command `git push <remote> <commit-reference>`. If you followed Github's
instructions when creating your repository, the remote will be named `origin`,
meaning the previous command becomes `git push origin <commit-reference>`.
There are several ways to specify a commit to push the remote. The most
common way is to use the name of a branch: the command `git push origin main`
will push the last commit of the `main` branch to the `main` branch in the
remote, bringing the remote up to date with your local repository. Other
possible ways to specify a commit include using the `HEAD` reference, which
identifies the commit you are currently viewing, and using the unique hash
identifier for a commit.

If you are working on a project with others or are updating it from two different
local copies of the repository (e.g. one copy on a work laptop and one on ocean),
it is possible that the remote repository will have commits stored that you do
not have in your local repository. You can bring your local repository up to date
with the remote by using the command `git pull` or `git fetch`. These commands will
download all updates from the remote repository to your local copy. The `git pull`
command will also merge these updates into your local files, essentially saving
over your local files with those in the remote (to a first approximation). Normally,
this is exactly what you want. However, it also means that if you have any local
file changes that are not yet stored in a commit, using `git pull` will write over
those uncommited edits. See the section below about collaborative editing below for
more information.

Since each commit builds on the previous commits in the project history, pushing
a commit to the remote will also push all intermediate commits between the current
state of the remote and the specified commit. Similarly, pulling a commit from the
remote will also pull all intermediate commits between the state of your local
repository and the commit on the remote. See the section about visualizing the
project history if you need help thinking about this.

## Git branches and collaborative editing

When you are the only person working in a repository and are simply using Github
as a backup system, the above information should be enough to keep you out of
trouble. However, if you are working in a repository with multiple authors (which
includes yourself working from multiple devices!), there are a few extra
considerations to pay attention to.

### Potential pitfalls

The main issue that can arise in this situation is when multiple authors have
edited the same file in a repository and made separate commits which are pushed or
pulled to the same local or remote repository. Git is fairly smart about handling
this if the edits are in different parts of the file, but if several authors have
edited the same line(s) then human intervention is needed to untangle the situation.
Git can help with this through the `git merge` command, which I strongly encourage
reading up on if you think this situation may apply to you. Briefly, this tool will
display the conflicting edits side by side so that you can edit the file to best
incorporate both changes, and then create a new merge commit.

Another potential issue that can arise is when you have uncommitted local changes
to files and want to use `git pull` to update your local repository with commits
from the remote. Using `git pull` in this situation will potentially overwrite
your local uncommitted changes, resulting in lost work. If you are in this
situation, you should commit your local changes, run `git fetch`, and then run
`git merge` to incorporate the changes from the remote into your local files.
To avoid this situation, it is good practice to never leave local changes
uncommitted, and to always run `git pull` *before* starting to edit a shared branch.

### Best practice workflow

Programmers have developed a common practice workflow for shared repositories to
minimize the occurrence of issues like these. The core of this workflow is that the
main branch of a project is rarely directly edited, if ever. Instead, a programmer
will create a new branch when developing a new feature, which can then be merged
into the main branch once the feature is complete.

        A--B (main)
            \
             E--F--G (newbranch)

As other people complete features and merge their branches into `main`, the two
branches will now have distinct histories:

        A--B--C--D (main)
            \
             E--F--G (newbranch)

Once you complete work on the new branch, you can switch back to the main branch,
update your copy using `git pull`, and use `git merge newbranch` to incorporate the
changes from `newbranch` into the main branch.

        A--B--C--D---H (main)
            \       /
             E--F--G (newbranch)

This way you only have to merge once, and there's a much lower chance of someone
writing over your work or you writing over theirs.

To create a new branch, use the command `git branch <branchname>`. This creates
the new branch, but it does **not** switch you to the new branch! You can see this
by running `git log` or similar and looking at where `HEAD` is pointing (`HEAD`
indicates your current view of the project history). At this point, the contents
of the files in the original branch and the new branch will be identical. To switch
to the new branch, run the command `git checkout <branchname>`. Note the changed
position of `HEAD` when you run `git log`. From this point on, any new commits
will be stored in the new branch instead of the original branch. Switching back and
forth between branches with `git checkout` will "magically" update the contents of
the files so that they reflect the current state of that branch.

# Visualizing the project history

You may want to visually represent the chain of commits in your project's history.
This can be accomplished with the `git log` command. This will display a list of
the commits in the repository, including for each commit 1) the unique hash
identifier, 2) the name and email of the commit's author, 3) the date and time of
the commit, and 4) the associated commit message. However, this takes up a lot of
space and you often won't need to see all that detail. The next section contains
some aliases that might be useful for condensing the log output into a more legible
form.

You may also notice odd-looking parentheticals in the log output like `(HEAD -> main)`
or `(origin/main)`. These indicate various reference points that Git uses to track
your position in the project history. The `HEAD` reference indicates your current
position in the project history. If you have multiple branches in your project, the
current state of each branch will be marked with the branch name (e.g. `main`) and
`HEAD` will indicate which branch you are currently viewing. References like
`origin/main` indicate the state of the remote repository (e.g. the latest commit
on the `main` branch which exists on the remote `origin`).

# Useful Git aliases

For common operations, you may want to define aliases in Git. Here are a few that I
find useful.

* `git config --global alias.co checkout`

* `git config --global alias.br branch`

* `git config --global alias.ci commit`

* `git config --global alias.st status`

* `git config --global alias.cm=commit -m`

* `git config --global alias.aa=add -A .`

    This alias adds all files in the current directory to Git to be tracked and/or stages them for the next commit.

* `git config --global alias.ca=commit -a`

    This alias commits all modified files, whether or not they were already staged for a commit.

* `git config --global alias.cam=commit -a -m`

* `git config --global alias.df=diff`

    This alias shows the unstaged changes from the previous commit.

* `git config --global alias.dfc=diff --cached`

    This alias shows the staged changes from the previous commit.

* `git config --global alias.dfh=diff HEAD`

    This alias shows all changes (staged and unstaged) from the previous commit.

* `git config --global alias.poh=push origin HEAD`

    This alias pushes the current state of the current branch to Github.

* `git config --global alias.pom=push origin main`

    This alias pushes the current state of the main branch to Github.

* `git config --global alias.ls=log --pretty=format:"%C(yellow)%h%Cred%d %Creset%s%Cblue [%cn]" --decorate=short --graph`

    This alias reformats the output of `git log` to be more condensed and readable.

* `git config --global alias.ll=log --pretty=format:"%C(yellow)%h%Cred%d %Creset%s%Cblue [%cn]%n%Creset%b" --decorate=full --numstat`

    This alias is similar to the `ls` alias above, but it also lists which files were modified in each commit and other details.

* `git config --global alias.la=!git config -l | grep alias | cut -c 7-`

    This alias lists all existing git aliases.

For example, if you set the first alias in the list, then typing `git co` in
your terminal will be equivalent to typing `git checkout`.

# Updating instructions

To the poor soul updating this: edit the .md file with a text editor, then render:

`pandoc -t latex -f gfm -V colorlinks=true -V title="Intro to Git and Github" -V geometry="margin=1in" --toc -o "Intro to Github.pdf" "intro-to-github.md"`
