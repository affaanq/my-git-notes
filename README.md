# my-git-notes
A simple repository where I am writing all the things I learned from "Frontend Masters: Git in-Depth"

Porcelain in git: It is the interface, the command that you type in the terminal to interface with Git
Plumbing in git: How git works under the hood

Git: It is a Distributed Version Control System

Git stores the compressed data in a blob, along with in a header:
the identifier blob
the size of the content
\0 delimiter, string terminator
content

Git stores the object in .git directory

blob is stored in the objects directory

I dentical content in Git only is stored ones, it also does not store an empty directory.

Commit Objects:

it point to the tree
contains autohr, date, message, and the parent commit
A commit is a coe snapshot 
types of objects: Blob, Tree and Commit

The three areas of coding in Git

The Working Area
The Staging Area: files going to be the part of the commit
The Repository: The files git knows about and which contains all the commit

#The Staging Area

GIT ADD -P: it allows you to stage commits in chunks, interactively

Git stash is to save your un-committed work