---
title: Cutting down the size of your repo
# date: 2018-10-29 00:00 +00:00
header: 
  overlay_image: /assets/images/pages/teaser-default.jpg
  teaser: /assets/images/pages/teaser-default.jpg
  caption: Me enjoying the sunny Yellowstone
categories: 
  - Professional
tags: 
  - git
  - technology
---

Latterly I've been playing with Git and trying to fix a couple of size problem with more or less luck. Git, is a wonderful tool to track and follow changes in almost any kind of document/s or project, specially is they are plain text documents —or sort of— like any kind of code is. Problem is, code isn't always alone and oftentimes it goes in company with other files, mostly binary files that the code uses or produces. Those files can still be tracked with Git, but Git usually can't see through them since they aren't text. So, you only know that the file has changed and you record the whole size changing[^1]. If you have big files and / or multiple small files that change could be a problem since every time you change —or move— the file, you duplicate —more or less— the size of the file in the repo since Git is keeping a copy of the old file plus the new file. 

In other cases, you are a little bit clumsy or inexperience with Git —like me— and you commit files that are too big for GitHub —more that 100mb— and when you want to push the changes to GitHub you can't. 

## Measuring your repo

I think the first thing before we start reducing the size of our repo is measuring it the real size or it and where it the problem. For that we can use a tool called [`git-sizer`](https://github.com/github/git-sizer) and it's going to give us a report of why our repo is so big. To install it: 

```shell
$ brew install git-sizer
```

Then you go got the root of your repo and you can type: 

```shell
$ git-sizer --verbose
Processing blobs: 7865
Processing trees: 7315
Processing commits: 2974
Matching commits to trees: 2974
Processing annotated tags: 96
Processing references: 115
| Name                         | Value     | Level of concern               |
| ---------------------------- | --------- | ------------------------------ |
| Overall repository size      |           |                                |
| * Commits                    |           |                                |
|   * Count                    |  2.97 k   |                                |
|   * Total size               |  1.16 MiB |                                |
| * Trees                      |           |                                |
|   * Count                    |  7.32 k   |                                |
|   * Total size               |  5.17 MiB |                                |
|   * Total tree entries       |   131 k   |                                |
| * Blobs                      |           |                                |
|   * Count                    |  7.87 k   |                                |
|   * Total size               |   281 MiB |                                |
| * Annotated tags             |           |                                |
|   * Count                    |    96     |                                |
| * References                 |           |                                |
|   * Count                    |   115     |                                |
|                              |           |                                |
| Biggest objects              |           |                                |
| * Commits                    |           |                                |
|   * Maximum size         [1] |  11.2 KiB |                                |
|   * Maximum parents      [2] |     2     |                                |
| * Trees                      |           |                                |
|   * Maximum entries      [3] |   573     |                                |
| * Blobs                      |           |                                |
|   * Maximum size         [4] |  10.4 MiB | *                              |
|                              |           |                                |
| History structure            |           |                                |
| * Maximum history depth      |  1.99 k   |                                |
| * Maximum tag depth      [5] |     1     |                                |
|                              |           |                                |
| Biggest checkouts            |           |                                |
| * Number of directories  [6] |   104     |                                |
| * Maximum path depth     [6] |     9     |                                |
| * Maximum path length    [7] |   105 B   | *                              |
| * Number of files        [8] |  1.90 k   |                                |
| * Total size of files    [8] |   177 MiB |                                |
| * Number of symlinks         |     0     |                                |
| * Number of submodules       |     0     |                                |
```

and you will get something similar to that one... 

Now, you know that the problem is your blobs most of the weight of your repo. 

## When you commit files too big for GitHub

I think this is the most common case when you are beginning with Git and even more with GitHub. You are happily committing your changes with Git and when you wan to push them to GitHub to share your repo or to have a backup of it, you come across a message saying that some files are *too fat* for GitHub and you can't upload. 

Well, this is more or less easy to fix and you can use two tools to do so. One is an "in house" tool and it called [`git-filter-branch`](https://git-scm.com/docs/git-filter-branch). The other one is an external tool and everybody says it's easier to use and more effective, [BFG Repo-Cleaner](https://rtyley.github.io/bfg-repo-cleaner/). I decided to use the latter one since it seems simpler, faster and easy to use. 

You can find more detailed instruction in the [BFG Repo-Cleaner website](https://rtyley.github.io/bfg-repo-cleaner/#usage), but this is more or less what I've done. First, you install the tool in your machine. 

```shell
$ brew install bgf
```

Then... I would do for security clone a fresh copy of your repo or duplicate it in your hard drive. 

**Be careful:** do a copy of your repo before you work with and keep that copy until you are really sure everything work as intended. 
{: .notice--danger}

Then you can go to root of your repo or from outside run the following command that will delete all the blobs from the commits of your repo bigger than 100 Megabytes. 

```shell
$ bfg -b 100M [some-big-repo.git]
```

\* You need to add the last part if you are outside the repo
{: style="font-size: small"}

BFG doens't really delete the blobs from the repo, just from the commits. So after you've run BFG you need to do housekeeping in your repo with the following commands. 

```shell
$ git reflog expire --expire=now --all && git gc --prune=now --aggressive
```

[`reflog`](https://git-scm.com/docs/git-reflog) command manage the references log and [`gc`](https://git-scm.com/docs/git-gc) clean your repo of unnecessary stuff —garbage collector. 

Now you have to notice the decrease in size in your repo and you can push your changes now. 

You need to take into account a couple of things before you push.

- If you have follow the BFG website steps you probably have cloned you repo from some online repository, and now you can `push`.  
- If you don't, you are going to get message something like you first have to `pull` and then you are going to `push`. You can use `-f` to force the push if you want. 
- This happens basically because when you are using BFG you are rewriting your history and in consequence you're changing the hash of your commits, so they don't match anymore with the commits in your online repo. In other words, you need to ditch everything you have upstream and upload your repo as it was new. For these reason you need to use `—force` to upload. 
- This will have consequences if you are sharing your repo with other users or if you have merge your repo with other repos. 

**Again be careful:** and save a copy of your previous work till you know everything work properly. 
{: .notice--danger}

## BFG When you've merged with an upstream repo that you don't control

I'm going to put as example what I've done with the repo of this blog. I didn't have a lot of experience with Git —and I still don't, living and learning— and I was sometimes just trying things here and here. The result has been a a little bit clutter Git history to what you have to add that I've moved my images a couple or times till I settle with location I like. I also optimize them with a little app, so the end result is I have in my tree those images committed perhaps a minimum of couple of times, and sometimes three or four times. 

The original repo of the template has around ~70mb is size and my repo reached around ~500mb on my hard drive and around ~230mb in GitHub. I've made changes and added photos, but not that much to such a bit size. 

As I told you, when I started this blog, my use of Git was a little bit rudimentary and didn't used branches properly, committed a lot in the master branch and other stupidities. I was putting *patches* and trying things, and I even some time I think i duplicated all my history and they fix... Summing up, [no sleep stories](https://www.reddit.com/r/nosleep/), that almost all of us have suffered when you are learning the ropes of a new tool. 

On top of all of that, I wanted to continue to receive updated of the template from his creator repository, so at some point I set up a branch which upstream was the master of minimal mistakes template. Usually my workflow was: 

pull the changes → merge to my develop branch → check everything is to my liking → merge to master
{: .text-center}

This setup gives me total control about what is and how the template is updated while I still receive updates. It's a little bit more manual than have the gem[^2] and overlap what I don't like or want to customize with my code, but I can do thing more granular and I can learn in the process —editing the code and seen from inside how minimal mistakes changes things. 

So, I run BFG in this repo in a aggressive way, looking to everything over 500K and even deleting all the image files in the folder `/assets`. 

The result was gorgeous. I really trimmed the size to what I have to be —around ~140mb. 

### The problem

The problem doing this was... I unrelated the histories of my repo with the minimal mistakes repo, in other words I have to `--alow-unrelated` as I did the first time I wanted to merge them again[^3]. This happens basically, because as I mentioned above, BFG rewrites your history and to do so you 



[^1]: This is not really true is you use some Git GUIs or Diff tools that can show you the two different versions of the file, but in the end you are going to to end with the two versions stored somewhere else.
[^2]: I have to confess that till really recently I was using the gem and the whole code of the template at the same time. I didn't have any downside, but it wasn't really smart.
[^3]: You are probably thinking: *didn't you make a branch to begin with you changes form the original template?*. NO! :scream: I didn't. I just delete all the stuff I didn't wanted and then I begin to edit the template. After I changed to upstream repository of master to the curren repository —actually I was messing even more for sake of curiosity, but for the sake of everyone let's keep this sort— and uploaded my changes. After, that I realized I wanted updates and after more messing, so on an so forth, I ended with a branch with the upstream to minimal mistakes master.

