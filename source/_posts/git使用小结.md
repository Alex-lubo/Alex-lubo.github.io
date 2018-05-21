---
title: git使用小结
date: 2018-02-17 22:24:09
tags:
---
## install git 

```
sudo add-apt-repository ppa:git-core/ppa
sudo apt-get update
sudo apt-get install git
```

## config git

1. generate ssh
```
ssh-keygen -t rsa -C "name@company.com"
```
2. add sshkey
copy the rsa_pub to remote repository

3. config name and mail
```
git config --global user.name "name"
git config --global user.email "name@company.com"
```

## using git
1. get remote branch to local:

```
git checkout --track origin/branch-name
```

The command will create a local branch named branch-name ,tracking the remote branch origin/branch-name.

2. track is shortband for 

```
git checkout -b [branch] [remotename]/[branch]
```

If you are trying to "checkout" a new remote branch (that exists only on the remote, but not locally), here's what you'll need:

```
git fetch origin
git checkout --track origin/<remote_branch_name>
```

3. delete local branch and remote branch
```
# delete branch local
$ git branch -D feature/abc

# delete remote branch
$ git push origin --delete feature/abc
```
**Note**: local branch has nothing to do with remote branch, they are completely separate objects.