---
layout: default
title: Git Basic
---

# Git Basic

## Useful Commands

**Clone a repository from local path to dest path**

```bash
git clone -l -s local/repo dest/repo
```
    
**Checkout a remote branch**

```bash
git checkout orign/dev dev
```

**Checkout a new branch and track with remote branch**

```bash
git checkout -b dev origin/dev
```
    
**Revert uncommit file/dir**

```bash
git checkout file_name/dir_name
```

**Check last two changes**

```bash
git log -2
```

## Useful References

  * [Undoing in Git - Reset, Checkout and Revert](http://book.git-scm.com/4_undoing_in_git_-_reset,_checkout_and_revert.html)
  * [A successful Git branching model](http://nvie.com/posts/a-successful-git-branching-model/)

## Git Autocompletion

1. edit ~/.bash_profile, if the file not exits, create a new one.

2. add the following script in this file:

    ```bash
    # Set git autocompletion and PS1 integration
    if [ -f /usr/local/git/contrib/completion/git-completion.bash ]; then
        . /usr/local/git/contrib/completion/git-completion.bash
    fi
    GIT_PS1_SHOWDIRTYSTATE=true
    
    # if you install git through MacPort
    if [ -f /opt/local/etc/bash_completion ]; then
        . /opt/local/etc/bash_completion
    fi
    
    # if you install git through homebrew
    if [ -f `brew --prefix`/etc/bash_completion.d/git-prompt.sh ]; then
        . `brew --prefix`/etc/bash_completion.d/git-prompt.sh
    fi
    if [ -f `brew --prefix`/etc/bash_completion.d/git-completion.bash ]; then
    . `brew --prefix`/etc/bash_completion.d/git-completion.bash
    fi
    
    # PS1='\[\033[32m\]\u@\h\[\033[00m\]:\[\033[34m\]\w\[\033[31m\]$(__git_ps1)\[\033[00m\]\$'
    PS1='\[\033[32m\]\u@\h\[\033[00m\]:\[\033[34m\]\w\[\033[31m\]\[\033[00m\]\$'
    ```

3. reboot Terminal (Command+Q, then open it again)

4. Now when you enter a git project in Terminal, you will see:

    ```bash
    your_name:~/your/git/path (your_branch[status]) $
    ```

    You can use the auto-completion by hitting `<tab>`.

## My Git Configuration

```bash
git config --global color.branch auto
git config --global color.diff auto
git config --global color.status auto
```

## Reference 

  * [official site](http://git-scm.com): this is the official site.
  * [offical faq](https://git.wiki.kernel.org/index.php/GitFaq): a very useful official FAQ.
  * [github](https://github.com): github is always your friend!
  * web client
    * gitWeb: the CGI tools for git.
    * [All web app for git](https://git.wiki.kernel.org/index.php/Gitweb)
    * [cgit](http://hjemli.net/git/cgit/log/)
    * [gitalist](http://example.gitalist.com/)
  * server
    * [gitolite](https://github.com/sitaramc/gitolite)
  * solution
    * [Git Lab (a free github like solution)](http://www.gitlabhq.com/)
    * [Github Enterprise (about $5,000 per year)](https://enterprise.github.com/)
    * [gitoriours (about $5,000 for local install)](http://gitorious.org/projects/gitorious)
  * GUI Tools
    * [gitx (Mac OSX)](http://gitx.frim.nl/seeit.html)
      * [[https://github.com/pieter/gitx]]
    * [Git Extensions](http://code.google.com/p/gitextensions/)

## Articles

**Books**
  
  * [Pro Git](http://progit.org/book) is a good start for you to use git, and here is a [Chinese](http://progit.org/book/zh) version.  
  * [Git Community Book](http://book.git-scm.com/index.html)

**Tools**

  * [Gerrit](http://code.google.com/p/gerrit/): a code review web tool used for git based projects.

**Comparison**

if you want to compare git with the other source control system, these articles could help you choose a proper one:

  * [Mercurial vs Git](http://rg03.wordpress.com/2009/04/07/mercurial-vs-git): hg or git, read it to make your decision.
  * [Why Git is Better than X](http://whygitisbetterthanx.com) also available in [Chinese](http://zh-tw.whygitisbetterthanx.com/#git-is-fast)

**Tutorial**

  * [Understanding Git Conceptually](http://www.eecs.harvard.edu/~cduan/technical/git): another useful tutorial, also cover some theory behind git.
  * [git ready](http://www.gitready.com): git ready is a site contents many useful commands in operating git.

**Work with svn**

When you finish import git from svn, becarefule of the CRLF problem. To solve this in windows, 
you need rewrite all the files so that they become CRLF in windows, then commit push whole project to the server. 
that will trigger auto CRLF convert in git. If you forget do this and start using git, each time you commit files, 
the whole project will changed, and you will lose your diff-list. 

  * [Cleanly import svn repository into git](http://djwonk.com/blog/2008/05/09/cleanly-import-svn-repository-into-git): this is the method I used when I convert my project from svn.
  * [Guides: Import from Subversion](http://github.com/guides/import-from-subversion)

**Host git server ( why git need a server ?? )**

Hosting git server is painful for Windows computer, I doubt most people would choose [github](https://github.com) instead. 
But I do have some private project which need fast access from local machine, and most Chinese people are suffer the speed 
connect to github :(. So these documents shows you how to craete your own git server in your computers. It bring the old school 
svn-like method back to you :).

  * [Hosting Git repositories, The Easy (and Secure) Way](http://scie.nti.st/2007/11/14/hosting-git-repositories-the-easy-and-secure-way): This is what I done in my local pc. (need you to have two PCs, host is linux). 
  * [Git Server: Gitosis and Cygwin on Windows](http://www.markembling.info/view/git-server-gitosis-and-cygwin-on-windows): I try this, but failed in my Windows computer, hope it works for you.

**SSH**

For people who don't know ssh too much, these documents help you create your ssh-key so that you can work on git.

  * [基于公钥认证方式的 OpenSSH Server 自动登录完全手册(Linux/Windows 下的 SSH 自动登录指南)](http://rainux.org/openssh-public-key-authentication-guide-automatic-login)

**Issues**

Some common issues in using git

  * [git-svn Tip: don't use core.autocrlf](http://weierophinney.net/matthew/archives/191-git-svn-Tip-dont-use-core.autocrlf.html)
  * [Issue 158: Pull modifies files (CRLF conversion issue)?](http://code.google.com/p/msysgit/issues/detail?id=158)

**Tips**

  * [Git Tip: How to Merge Specific Files from Another Branch](http://jasonrudolph.com/blog/2009/02/25/git-tip-how-to-merge-specific-files-from-another-branch)
  * [Show Git Branch in Bash Terminal](http://railsnotes.com/603-show-git-branch-in-bash-terminal): you like the hint in git-mingw don't you? this is how it works in bash (linux).

## FAQ

**Q1: How to create new project controlled by gitosis**

add project group in gitosis-admin/gitosis.conf, for example, your want to add "johhny" in the develop team of foobar project

```bash
[group foobar-devteam]
writable = foobar
members = johnny@computer_name
```

commit the changes using git commit, and push it to the server.  go to the foobar project local directory

```bash
cd foobar
git init
git remote add origin git@your_server:foobar.git
# ...
# do some work, commit files 
# ...
git push origin master:refs/heads/master
```
    
**Q2: How to remove git repository from gitosi**

in your server, check /home/git/repositories/ and remove your_project.git from it.

**Q3: What’s the difference between git pull and git fetch?**

http://stackoverflow.com/questions/292357/whats-the-difference-between-git-pull-and-git-fetch 

In the simplest terms, "git pull" does a "git fetch" followed by a "git merge".
One use case of "git fetch" is that the following will tell you any changes in the remote branch since your last pull... 
so you can check before doing an actual pull, which could change files in your working copy.

```bash
git fetch
git diff ...origin
```

**Q4: How to check remote branch and local branch relationship**

```bash
git remote show origin 
```

Here origin is the alias of the remote.

**Q5: How to erase unwanted change-list (carefully use it!)**

```bash
git reset --hard (commit) # in your local machine, reset to the version you want.
git push --force # this will force push your HEAD pointer and replace the remote HEAD pointer  to it.
```

**Q6: How to remove remote branch**

```bash
# the push syntax is "git push refs/tags/<tag>:refs/tags/<tag>", 
# in short it could be "git push <src>/<dst>".
# in git document, it said: 
# Pushing an empty <src> allows you to delete the <dst> ref from the remote repository.
git push origin :branch_name
```

**Q7: How can I fast-forward branch A when I'm currently in branch B**

```bash
# suppose I'm in branch_B now.
# if branch_A can be fast-forward, we can use the command below manipulate branch_A without
# switching branch.
jwu@mac:~/foo/bar (branch_B)$ git pull origin branch_A:branch_A
```

**Q8: How can I merge two or more commit into one?**

```bash
git rebase -i HEAD~4 # squashing last four commits into one.
```

check this document for more details: http://www.gitready.com/advanced/2009/02/10/squashing-commits-with-rebase.html

**Q9: How to process filenames have white-space in it?**

You should use "\ " instead of directly typing the whitespace for the file.

suppose we got the message below:

```bash
# Changed but not updated:
#   (use "git add/rm <file>..." to update what will be committed)
#   (use "git checkout -- <file>..." to discard changes in working directory)
#
#	deleted:    Assets/user/nantas/Texture/Materials/gridred 1.mat

to delete file "gridred 1.mat", please type:

$git rm Assets/user/nantas/Texture/Materials/gridred\ 1.mat
```

**Q10: How to delete remote git tag?**

You probably won't need to do this often (if ever at all) but just in case, here is how to delete a tag from a remote Git repository.

If you have a tag named '12345' then you would just do this:

```bash
git tag -d 12345
git push origin :refs/tags/12345
```

**Q11: How can I merge the latest version without commit my current changes?**

use git-stash.

```bash
git stash
git pull
git merge another-branch
git stash pop
```
