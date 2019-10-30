---
title: Trash location in macOS vs Linux
date: 2017-10-26 19:37:42
categories:
  - Professional
  - Technology
tags:
  - linux
  - macOS
  - raspberry pi
  - raspbian
  - shell
gallery:
  - url: https://i.imgur.com/R7HwtwS.png
    image_path: https://i.imgur.com/R7HwtwS.png
    alt: "Trash-cli on macOS"
    title: "Trash-cli on macOS"
  - url: https://i.imgur.com/JZfTtYn.png
    image_path: https://i.imgur.com/JZfTtYn.png
    alt: "Trash-cli on Linux"
    title: "Trash-cli on Linux"
  - url: https://i.imgur.com/cfFEkHB.png
    image_path: https://i.imgur.com/cfFEkHB.png
    alt: "Trash on Raspbian"
    title: "Trash on Raspbian"
---

{% include figure image_path="https://i.imgur.com/pzcHnNy.png" atl="White trash (oh wait!) on macOS" caption="White trash (oh wait!) on macOS" %}{: .align-left}

[The other day](/blog/2017/10/19/trash-instead-of-rm/), I explained that there is a way to send files and folders to the trash from the the shell. Now, I just found out that there are differences between where is located the trash (it's a folder nonetheless) in macOS and Linux.

In macOS, the trash is located in your user's folder `rmtrash`   on macOS, since it's a [much nicer](https://github.com/PhrozenByte/rmtrash) and neat command. And easily to understand.

On another hand, in Linux, you can only use trash-cli (you can't get `rmtrash`   AFAIK), and I recommend you to install from source, as explained in it's [GitHub](https://github.com/andreafrancia/trash-cli) site, because if you install using `apt-get`  you are going to get a some kind of an old version without all the commands (`trash-restore`  was missing).

In Linux, you have two trashes when you are operating with `trash-cli`  on shell. One is your user's trash and you can access to it on [this](https://askubuntu.com/questions/102099/where-is-the-trash-folder) path `/home/$USER/.local/share/Trash` . There, you can see two folders files and info. I guest that you already know where are the files and where are the info / metadata of the files. However, if you use the command `sudo trash-put`  you files are going to be moved to `/root/.local/share/Trash` (obviously, or not that obvious as we've seen in macOS).

For the other `sudo`   before the command.

Now, you are ready to trash whatever you want.

{% include gallery %}
