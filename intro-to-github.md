# Intro to Git and Github

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

asdf

## Setting up a project repository

asdf

## Storing commits in Git and Github

asdf

### Git branches and collaborative editing

asdf

###

To the poor soul updating this: edit the .md file with a text editor, then render:

`pandoc -t latex -f gfm -V colorlinks=true -o "Intro to Github.pdf" "intro-to-github.md"`
