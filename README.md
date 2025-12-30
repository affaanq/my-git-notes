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