# Intro to Git and Github

This guide is a collection of the very basics of how to use Git and Github in your
work. If you find that you need additional information beyond this document, the
[Github documentation](https://docs.github.com/get-started) is quite good.

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
2. Set an email to attach to your git commits. Again, this doesn't have to be the
email connected to your Github account. In your terminal, run
`git config --global user.email "<email-address>"`
3. To enable helpful color in git output, run `git config --global color.ui auto`

If you want to work with remote repositories, you will first need to set up a
Github account and verify the email associated with it. Once you have done that,
you will need to set up credentials so that Github can verify your identity when
communicating with you over the command line. This process will look slightly
different depending on whether you use HTTPS or SSH as your secure channel with
Github.

## Setting up a project repository

asdf

## Storing commits in Git and Github

asdf

### Git branches and collaborative editing

asdf

###

To the poor soul updating this: edit the .md file with a text editor, then render:

`pandoc -t latex -f gfm -V colorlinks=true -o "Intro to Github.pdf" "intro-to-github.md"`
