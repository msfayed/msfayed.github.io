---
layout: default
title: From SVN to GIT
date: '2018-10-14T08:13:00.001-03:00'
author: Mohammed Fayed
tags:
- Bash
- GIT
- SSH
- svn
modified_time: '2020-04-28T10:04:16.244-03:00'
blogger_id: tag:blogger.com,1999:blog-3008009747231704115.post-8186719725264084597
blogger_orig_url: https://www.fayed.org/2018/10/from-svn-to-git.html
---


**First of all, we need to generate SSH keys for our machine:**

**Generate new SSH keys :**

'~' refers to your "home directory".
In Windows, you can find this by opening a command shell (cmd) and typing :

```shell
 echo %USERPROFILE%
```

Before continuing, check your `~/.ssh` folder (for example, `/home/jamal/.ssh` or `C:\Users\jamal\.ssh`) and look for the following files (existing SSH keys):


- id_rsa 
- id_rsa.pub 

if they are not exist, lets create new keys:

```shell
$ ssh-keygen -C "mohammed@fayed.org"
```

To remove the passphrase for the SSH key without having to create a new key (`NOT recommended`) :


```shell
 $ ssh-keygen -p
```

This will then prompt you to enter the keyfile location, the old passphrase, and the new passphrase (which can be left blank to have no passphrase).

If you would like to do it all on one line without prompts do :



```shell
 $ ssh-keygen -p [-P old_passphrase] [-N new_passphrase] [-f keyfile]
```

---

Make sure to change `TortoiseGit` SSH client to use Git SSH :



```
Right Click : TortoiseGit -> settings-> network -> SSH Client
set the value to : C:\Program Files\Git\usr\bin\ssh.exe
```

---

**Using Git Bash and to save the password for your SSH private key:**


Create ~/.bashrc , to create the bashrc file in windows: (ignore the error)


```shell
   copy > ~/.bashrc
```



Add the following lines to it :

```shell
   eval $(ssh-agent)
   ssh-add
```

--- 

**Convert SVN repository to GIT : **

Create a users file (i.e. users.txt) for mapping SVN users to Git it will be something like:




```shell
 user1 = First Last Name <email address.com="">
 user2 = First Last Name <email address.com="">
 ...
</email></email>
```

You can use this one-liner to build a template from your existing SVN repository (run it in Git Bash):



```shell
 svn log -q | awk -F '|' '/^r/ {sub("^ ", "", $2); sub(" $", "", $2); print $2" = "$2" <"$2">"}' | sort -u > users.txt
```

Now pull the SVN data from the repository:



```shell
 git svn clone --stdlayout --no-metadata --authors-file=users.txt svn://hostname/path dest_dir-tmp 
```

This command will create a new Git repository in `dest_dir-tmp` and start pulling the SVN repository. Note that the "`--stdlayout`" flag implies you have the common "`trunk/, branches/, tags/`" SVN layout.

If your layout differs, become familiar with `--tags, --branches, --trunk` options (in general git svn help).

All common protocols are allowed: svn://, http://, https://. The URL should target the base repository, something like `http://svn.mycompany.com/myrepo/repository`.

That must **not** include `/trunk, /tag or /branches`.

Note that after executing this command it very often looks like the operation is "hanging/freezed", and it's quite normal that it can be stuck for a long time after initializing the new repository. Eventually you will then see log messages which indicates that it's migrating.

Also note that if you omit the `--no-metadata` flag, Git will append information about the corresponding SVN revision to the commit message (i.e. `git-svn-id: svn://svn.mycompany.com/myrepo/`<branchname trunk="">`@`<revisionnumber> <repository uuid="">)

If a user name is not found, update your `users.txt `file then:

</repository></revisionnumber></branchname>
<branchname trunk=""><revisionnumber><repository uuid=""></repository></revisionnumber></branchname>

```shell
 cd dest_dir-tmp
 git svn fetch
```


You might have to repeat that last command several times, if you have a large project, until all of the Subversion commits have been fetched:

 


```shell
 git svn fetch
```


When completed, Git will checkout the SVN trunk into a new branch. Any other branches are setup as remotes. You can view the other SVN branches with:

 


```shell
 git branch -r
```


If you want to keep other remote branches in your repository, you want to create a local branch for each one manually. (Skip trunk/master.) If you don't do this, the branches won't get cloned in the final step.

 


```shell
 git checkout -b local_branch remote_branch
```


# It's OK if local_branch and remote_branch are the same name.

Tags are imported as branches. You have to create a local branch, make a tag and delete the branch to have them as tags in Git. To do it with tag "v1":

 


```shell
 git checkout -b tag_v1 remotes/tags/v1
 git checkout master
 git tag v1 tag_v1
 git branch -D tag_v1
```


Clone your GIT-SVN repository into a clean Git repository:

 


```shell
 git clone dest_dir-tmp dest_dir
 rm -rf dest_dir-tmp
 cd dest_dir
```


The local branches that you created earlier from remote branches will only have been copied as remote branches into the new cloned repository. (Skip trunk/master.) For each branch you want to keep:

 


```shell
 git checkout -b local_branch origin/remote_branch
```


Finally, remove the remote from your clean Git repository that points to the now deleted temporary repository:

```shell
 git remote rm origin
```



---
**To Clone into a BARE Git Repo**

```shell
 
$ git svn init https://path/to/you/svn repo-git --stdlayout 

#edit  repo-git/.git/config to contain:
[svn-remote "svn"]
        url = https://path/to/you/sv
        fetch = trunk:refs/heads/master
        branches = branches/*:refs/heads/*
        tags = tags/*:refs/tags/*

# also make sure that [ bare = true ]

$ cd repo-git
$ git svn fetch

```

---

**Set up an editor to work with GIT on Windows**

By executing

```shell
 $ git config core.editor notepad
```

users can now use notepad.exe as their default editor.

or to use Notepadd++ you can create .sh script file with this content

```shell
#!/bin/sh

"c:/Program Files/Notepad++/notepad++.exe" -multiInst -notabbar -nosession -noPlugin "$*"
```


and execute
```shell
$ git config --global core.editor C:/npp.sh
```

to test that your editor is working
```shell
$ git config -e
```


**Convert Local Git Repo to shallow**
```shell
$ git fetch --depth 10

Or

$ git pull --depth 1
$ git gc --prune=all

Or [ good trick for old git versions ]

$ git clone --mirror --depth=5  file://$PWD ../temp
$ rm -rf .git/objects
$ mv ../temp/{shallow,objects} .git
$ rm -rf ../temp

```
**Clean Git Repo**
```shell
 
$ git reflog expire --all --expire=now
$ git gc --prune=now --aggressive
```


Sources :
- [Azure DevOps Services : Use SSH key authentication](https://docs.microsoft.com/en-us/azure/devops/repos/git/use-ssh-keys-to-authenticate?view=vsts)
- [https://stackoverflow.com/a/3972103](https://stackoverflow.com/a/3972103)
- [https://stackoverflow.com/questions/10564/how-can-i-set-up-an-editor-to-work-with-git-on-windows](https://stackoverflow.com/questions/10564/how-can-i-set-up-an-editor-to-work-with-git-on-windows) 
- [https://stackoverflow.com/questions/38171899/how-to-reduce-the-depth-of-an-existing-git-clone](https://stackoverflow.com/questions/38171899/how-to-reduce-the-depth-of-an-existing-git-clone) 
- [https://stackoverflow.com/questions/12544318/why-git-svn-cannot-clone-a-bare-repo](https://stackoverflow.com/questions/12544318/why-git-svn-cannot-clone-a-bare-repo)

