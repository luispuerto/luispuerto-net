---
title: Normalizing line endings on files for Git
date: 2020-01-04 06:00:00 
# last_modified_at: 2019-00-00 00:00:00
header: 
 overlay_image: /assets/images/blog/2019/git-logo-01.png
 teaser: /assets/images/blog/2019/git-logo-01.png
 caption: Git logo
 overlay_filter: 0.3
categories: [Technology, Professional]
tags: [git, coding]
twitter: 
  image: /assets/images/blog/2019/git-logo-02.png
#   hashtags: [tag1, tag2] # Only for Twitter, they go before tags
# toc: true
# toc_label: "Unique Title"
# toc_icon: "heart"  # corresponding Font Awesome icon name (without fa prefix)
# toc_sticky: true
---

I had a repo that suffered an _unconventional fork_, in other works, someone decided copy it and work outside Git —or any other version control system :dizzy_face:. After that, the fork was set as a [Subversion][SVN] repo and store in a local server. To bring those changes back to the original repo, I had to set it up as a _non related_ branch of the original repo, as I explain here, so I can proceed and merge it later. 

In spite of that, and as if dealing with the changes to merge weren't enough, line endings on the subversion version —sorry for the redundancy— of the repo were showing `<0x0d>` when I performed a `git diff` of the two branches. [Line endings][1] are handled in a different way on _[Unix-like][]_ systems —like Linux or macOS— and on Windows systems. While Windows uses for line endings the `CR/LF`, convention that is also know as ASCII `0x0d` or `\r` and a newline `\n` which ASCII is `0x0a`, _Unix-like_ system uses just `LF`, also called `\n` or in ASCII `0x0a`. 

[Some people][2] argue that Windows way is more correct, because it uses a _new line_ and _carriage return_, while Unix it's just new line, which could be just that, new line but not the carriage return to the left most part of the line and in consequence you'd end at the end of a new line, instead of the beginning. The reality is, in Unix they decided to have just one character, so they could save some space in memory —which has all the sense in the world in the old days— and that's it. Since Git is a Unix developed app, if you have files with Widows line-endings and things haven't been configured properly, you end up with a bunch of files with something like this: 

```
-export(addconversionfactorandprice)<0x0d>
-export(adddot2logfile)<0x0d>
-export(addmissingmirrorflow)<0x0d>
-export(addpartnerflow)<0x0d>
-export(addregion)<0x0d>
-export(calculatediscrepancies)<0x0d>
-export(changecolumntype)<0x0d>
-export(changeflowmessage)<0x0d>
-export(checkdbcolumns)<0x0d>
-export(clean)<0x0d>
-export(clean2excel)<0x0d>
-export(cleancomext)<0x0d>
-export(cleancomextmonthly)<0x0d>
```

Nice... a lot of visual garbage around. And not even just visual garbage, but it also seems that `git diff` it's piking those differences in lines endings as real differences between files. So all the files that have been edited seem to have all those lines endings on all the lines and Git is assuming that the whole file was changed. 

## Where does the problem come from?

I really don't know where the problem come front, exactly, but I can have a guess. Most probably the _version-control-less_ fork was developed under Windows :scream: were changes to the line endings probably happened. However, that shouldn't be a problem since it isn't the first time I'm working on a repo that is used, and edited, on Windows and Unix-like systems and I never ever I have an issue like this. I guess that Git, on its normal operation on Windows, it's configured to strip those extra characters from the files when they are added to Git database. So, I guess, the problem comes from the Subversion repo and its transformation to a Git repo with the [`git-svn`][git-svn] command. Probably Subversion doesn't strip those characters off and I didn't have the correct configuration on Git to deal with this case the line endings weren't normalized.

## How to fix? 

Just a simple tip, don't use Windows :grimacing:, and the life of everyone would be much easier. Ok, I just kidding —or not— but since this is not possible let's see if we can find a real solution. 

### Git configuration

First of all, you have to [configure Git][3] accordingly, just in case. 

```shell
# for Windows
$ git config --global core.autocrlf true

# for Linux & macOS
$ git config --global core.autocrlf input
```

You can also establish configuration [at repo][4] level with the `.gitattributes` file with

```
* text=auto
```

### Transform current files

However, the above configuration will only prevent new files from having those line endings. How can you transform the previous files? You just can follow [GitHub tutorial][5] about it. 

1. You just first save your changes, in case you had any:                         

```shell
$ git add . -u
$ git commit -m "Saving files before refreshing line endings"
```

2. Use Git to renormalize everything in your repo
{: start="2"}

```shell
$ git add --renormalize .
```

3. Now you can see the renormailized files and commit them.  
{: start="3"}

```shell
$ git status
$ git commit -m "Normalize all the line endings"
```

Please be sure you are normalizing the correct branch. I had a lot of trouble because I thought I was normalizing the offending branch —I was not— and even after the normalization I was still seeing the line-endings garbage. 

You can also check this post on [stack over flow][6] if you want to know more. 

### dos2unix & unix2dos

If you don't want to use Git for this task, you can always rely in [`dos2unix` and on `unix2dos`][9] commands on terminal. If you don't have them, you can easily install with [brew][]. 

```shell
$ brew install dos2unix
```

and if you are in Linux, use whatever method is used in your distro. 

However, be careful using this apps because perhaps you _transform_ something that you aren't supposed to. Like for example, binaries. I did it and I had to restore them because I left then useless. 

## Bottom line

Guys, seriously, use Git, or subversion, or whatever version control you like to version your work and make it easy to others —and to yourself— to figure out what was going on your work and on your code. I really don't understand why this is not a common practice when people is teaching / learning to program in whatever language. It's one of the things that make your work look like a pro. 

Also be mindful about the system / OS you are using and how it works. Not everyone is using the same OS and we need to make things work cross-platform. 



[1]: https://community.perforce.com/s/article/3096
[2]: https://stackoverflow.com/a/1552775/6888648
[SVN]: https://en.wikipedia.org/wiki/Apache_Subversion
[3]: https://www.git-scm.com/book/en/v2/Customizing-Git-Git-Configuration#_code_core_autocrlf_code
[4]: https://help.github.com/en/github/using-git/configuring-git-to-handle-line-endings#per-repository-settings
[5]: https://help.github.com/en/github/using-git/configuring-git-to-handle-line-endings
[6]: https://stackoverflow.com/a/15646791/6888648
[8]: https://en.wikipedia.org/wiki/Version_control
[Unix-like]: https://en.wikipedia.org/wiki/Unix-like
[git-svn]: https://git-scm.com/docs/git-svn
[9]: https://www.lifewire.com/dos2unix-linux-command-4091910
[brew]: http://brew.sh
