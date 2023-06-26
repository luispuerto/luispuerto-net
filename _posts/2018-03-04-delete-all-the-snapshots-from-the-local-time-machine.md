---
title: Delete all the snapshots from the local time machine
date: 2018-03-04 13:53:42
header: 
  overlay_image: assets/images/blog/2018/240086966_1f04572a86_z.jpg
  teaser: assets/images/blog/2018/240086966_1f04572a86_z.jpg
categories:
  - Technology
tags:
  - how-to
  - macOS
---
Sometimes you need to delete all the snapshots that the macOS time machine do locally just because you want to get rid of something of because you need to reclaim that space. Before one could use `sudo tmutil disablelocal` to delete everything and then `sudo tmutil enablelocal` to enable the local snapshots again —since they are useful. However, that option is no longer available.

So if you run

```shell 
sudo tmutil listlocalsnapshots /
com.apple.TimeMachine.2018-03-03-144538
com.apple.TimeMachine.2018-03-03-154712
com.apple.TimeMachine.2018-03-03-170903
com.apple.TimeMachine.2018-03-03-181447
com.apple.TimeMachine.2018-03-04-094245
com.apple.TimeMachine.2018-03-04-110835
com.apple.TimeMachine.2018-03-04-121348
```

You probably end up with a similar list as mine, or perhaps even longer. If you waned to free some space, you need to use the command `tmutil deletelocalsnapshots` and introduce the date os each of the snapshot one by one to delete them, which it's quite a pain in the \***.

Lucky, [someone in the MacRumors](https://forums.macrumors.com/threads/how-to-delete-time-machine-local-backups-on-high-sierra.2073998/#post-25673423) forum came with a clever idea of using the terminal and the grep command. If you run:

```shell 
tmutil listlocalsnapshotdates / |grep 20|while read f; do tmutil deletelocalsnapshots $f; done
Deleted local snapshot '2018-03-03-144538'
Deleted local snapshot '2018-03-03-154712'
Deleted local snapshot '2018-03-03-170903'
Deleted local snapshot '2018-03-03-181447'
Deleted local snapshot '2018-03-04-094245'
Deleted local snapshot '2018-03-04-110835'
Deleted local snapshot '2018-03-04-121348'
```

You are going to get rid of all those snapshots in no time.

Learn how to use the terminal commands kids… it's going to say
