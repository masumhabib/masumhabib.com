Git for Ph D students
#####################
:date: 2012-12-19 21:20
:category: Tutorial
:slug: git-for-ph-d-students
:tags: git
:status: draft

Git version control system for Ph D students

| Global settings
|  $ git config --global user.name "John Doe"
|  $ git config --global user.email johndoe@example.com
|  $ git config --global core.editor vim
|  $ git config --global merge.tool vimdiff

| Check settings
|  $ git config --list

| Get a single setting
|  $ git config user.name

Help

| $ git help 
|  $ git help config

| To start a project with version control:
|  $ cd /path/to/project
|  $ git init

| Add all the files in current directory:
|  $ git add .

| Commit the initial version:
|  $ git commit

| Cloning
|  $ git clone usename@host://path/to/repo/

| Branch: creating -
|  $ git branch new-branch-name

| Push
|  $ git push origin new-branch-name

| Merge and update server
|  $ git checkout master
|  $ git merge new-branch-name

| Fetching changes from remote
|  $ git remote add repo-name usename@host://path/to/repo/
|  $ git fetch origin

or

$ git fetch repo-name
