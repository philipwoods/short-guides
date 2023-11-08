# Intro to Git and Github

This guide is a collection of the very basics of how to use Git and Github in your
work. If you find that you need additional information beyond this document, the
[Github documentation](https://docs.github.com/get-started) is quite good. Github
also created a very helpful
[Git cheat sheet](https://training.github.com/downloads/github-git-cheat-sheet/)
for quick reference.

Note that this guide describes how to use Git and Github to track and back up
your own work. If all you want is to download someone's existing code from Github,
you can simply use the command `git clone <url>"` where the url of the desired
repository can be found in the green 'Code' dropdown menu on the repository's
Github website.

Because of its common usage in scientific computing, all example commands provided
in this guide assume a UNIX-based operating system such as Linux or MacOS.

If you have questions about any of this information and the documentation above
isn't clear, feel free to ask me (Philip) for help.

## The basics

### What is Git?

Git is a version control system which allows you to retain not just the final version
of your project, but all intermediate versions along the way. This capability is like
non-destructive editing, where each change you make can be reverted, but Git also
provides some additional functionality.

Git accomplishes this by storing information about your project at commit points which
you identify in a repository. At each commit point, Git stores a list of changes made
to the project, the author of those changes, and a message describing the contents of
the changes. These commit points are linked together to create a (possibly branching)
history of the project.

### What is Github?

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

## Initial setup of Git/Github

### Installing and configuring Git

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

### Setting up secure connections with Github

If you want to work with remote repositories, you will first need to set up a
Github account and verify the email associated with it. Once you have done that,
you will need to set up credentials so that Github can verify your identity when
communicating with you over the command line. This process will look slightly
different depending on whether you use HTTPS or SSH as your secure channel with
Github, but the end result will be essentially the same either way. You will need
to remember which authentication protocol you chose, because the URL referring to
a Github repository is slightly different when using HTTPS or SSH.

#### Connecting with HTTPS

I haven't used HTTPS very much to communicate with Github, so take this with a
grain of salt. My understanding is that you simply use your normal Github account
credentials to authenticate this way, but Github may be removing this in favor of
personal access tokens. These personal access tokens are more secure in that you
can choose the priveleges associated with each token, but this may not be necessary
for typical users. For more information about setting up personal access tokens,
see the guide [here](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens).

#### Connecting with SSH

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

## Setting up a project repository

In your Github account, click the ["New repository?"] button [near the upper right corner].
Choose a relevant name for the project, scroll down, and click the [button] to create
the repository. Github should display a page with two different sets of commands to
run in your terminal. If you have already created a Git repository on your local
machine and simply need to link it to Github, you should run the second set of commands.
Otherwise, run the first set of commands.

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

## Storing commits in Git and Github

As mentioned before, Git tracks changes to files by creating a chain of commits, each
of which is a snapshot of the history of the files in the repository at a particular
time.

Talk about git diff and git log

### Git branches and collaborative editing

asdf

## Useful Git aliases

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

* `git config --global alias.ls=log --pretty=format:"%C(yellow)%h%Cred%d\\ %Creset%s%Cblue\\ [%cn]" --decorate`

    This alias reformats the output of `git log` to be more condensed and readable.

* `git config --global alias.ll=log --pretty=format:"%C(yellow)%h%Cred%d\\ %Creset%s%Cblue\\ [%cn]" --decorate --numstat`

    This alias is the same as the above, but it also lists which files were modified in each commit.

* `git config --global alias.la=!git config -l | grep alias | cut -c 7-`

    This alias lists all existing git aliases.

For example, if you set the first alias in the list, then typing `git co` in
your terminal will be equivalent to typing `git checkout`.

###

To the poor soul updating this: edit the .md file with a text editor, then render:

`pandoc -t latex -f gfm -V colorlinks=true -o "Intro to Github.pdf" "intro-to-github.md"`
