---
title: "My MacBook Pro late 2011's discrete graphics card said \"ciao\" \U0001F44B\U0001F3FB —again"
date: 2017-12-05 11:51:30
header:
  overlay_image: assets/images/blog/2018/grey_screen.jpg
  teaser: assets/images/blog/2018/grey_screen.jpg
toc: true
categories:
  - Personal
  - Professional
  - Technology
tags:
  - dGPU
  - graphic-card
  - high-sierra
  - how-to
  - macOS
---

Last Sunday wasn't really a pleasant day. On Saturday late night, or rather around Sunday 00.30 am, my MacBook Pro late 2011's discrete graphic card begin to fail to in the end not being able to boot it properly. The computer was working just fine, it wasn't even using the discrete card, connected to the external screen as I've been doing lately, or any doing any other intensive task. I just rebooted it and 5' after loading the desktop a **solid gray screen** appear that allowed to do nothing. After I forced reboot pushing the on/off button, the normal loading **gray screen** had glitches as thin-horizontal-weird lines. After 3 or 4 boots into the desktop and then the **_gray screen of dead_** the computer begin to load directly just to the **_gray screen of death_**. Nothing could be done to load the computer normally. I tried safe mode, pressing `shift` key on boot, and nothing, just the same **_gray screen of death_**. Restore mode, `alt + R` on boot, also the **_gray screen of death_**. So, I decided to left the issue to sleep —it was around 1.30 am in the morning, and led the computer to make a hardware test, pressing `D` key on startup.

It's not the first time that the discrete graphic card fails in my laptop. It's a malaise that occurs to almost any 2011 Macbook Pro's computer. I think there is two kind of machines out there, the ones where the issue already happened and the ones where it's going to happen. My entire logic board was changed on July 2014 and 3 years and a half down the road it failed again last Sunday. Again, [**the same faulty chip**](http://appleinsider.com/articles/15/01/15/2011-macbook-pro-graphics-class-action-suit-expands-accuses-apple-of-concealing-defects). This is not proper quality Apple (AMD / Nvidia are also culprits here), and I don't know whose idea was to mount those logic board / chips on this Mac model, but I think it wasn't the brightest idea ever.

So, in Sunday morning, after I slept on the issue and I had a proper coffee and breakfast, I got my hands on to try to fix the problem. I begun with searching on internet about the problem and some people suggested that it was a RAM problem. Well, it could have been. The last time I did a hardware test, when I changed my hard drive in the US, it yielded a RAM problem, and this time threw me the same error.

{% include figure image_path="assets/images/blog/2018/IMG_4420.jpg" alt="Error 4MEM/66/40000000: 0x846b3798" caption="Error 4MEM/66/40000000: 0x846b3798" %}

So I decided to give it a try to this [guy's ](https://www.ifixit.com/Answers/View/142544/My+MBP+won%27t+boot%2C+with+horizontal+line+at+apple+logo#answer181366)suggestion and I even switched the RAM for the original ones, since I upgraded 4 years ago to two 8 GB modules. However, the result was always the very exact one. So I decided to ditch the idea that the RAM was the cause of the **_grey screen of death_,** reinstalled the RAM and looked for another solution.

Finally, I found these solutions ([1](https://forums.macrumors.com/threads/force-2011-macbook-pro-8-2-with-failed-amd-gpu-to-always-use-intel-integrated-gpu-efi-variable-fix.2037591/), [2](https://forums.macrumors.com/threads/disable-a-failed-amd-gpu-on-a-2011-macbook-pro-grub-solution.2087527/), [3](https://gist.github.com/blackgate/17ac402e35d2f7e0f1c9708db3dc7a44) and [4](https://github.com/blackgate/AMDGPUWakeHandler)), which were quite recent. It seems that a lot of boards are dying all over again. The solution is quite simple, since what is failing is the discrete graphic card and you usually don't use it in your daily life, you just have to stop using that card at all, and use all the time the integrated one. Problem solved. But, how do you make that happen when the only thing you are able to see is the **_grey screen of death?_** Basically you make the computer to not boot using the drivers for the discrete graphic car. The solution works, but there is some caveats, under High Sierra. One is that your Mac is not longer going to sleep properly, but it can be fixed just changing the sleep mode to hibernation. And that the bright control is not longer going to work, at least at the moment. I can live with that.

I'm going to summarize here what I've done to fix it under High Sierra macOS 10.13.1 on my Late 2011 MacBook Pro.

## The solution

First of all, I have to say that I begun with the solution number [1](https://forums.macrumors.com/threads/force-2011-macbook-pro-8-2-with-failed-amd-gpu-to-always-use-intel-integrated-gpu-efi-variable-fix.2037591/) and then I switch to the [2](https://forums.macrumors.com/threads/disable-a-failed-amd-gpu-on-a-2011-macbook-pro-grub-solution.2087527/), [3](https://gist.github.com/blackgate/17ac402e35d2f7e0f1c9708db3dc7a44) and [4](https://github.com/blackgate/AMDGPUWakeHandler) ones. Probably, you are fine to proceded from the solution number [2](/blog/2017/12/05/my-macbook-pro-late-2011s-discrete-graphics-card-said-ciao-again/#using-solutions-2-3-and-4) onwards. To make make those solutions work in the long run, you are going to need a USB stick in order to have a way of booting your Mac, from the very beginning, or when you update your system and the fix stop working as a consequence of the update. You don't need a very big USB stick. What you are going to store on it it's not going to more than 10 mb.

### Moving the kexts

This step isn't really necessary, and you can jump to the [next step](/blog/2017/12/05/my-macbook-pro-late-2011s-discrete-graphics-card-said-ciao-again/#using-solutions-2-3-and-4). However, this is the exactly how I did things, since first I found one solution and then the others. Also, if you have trouble booting with the other solutions, perhaps this part is going to help you.

You need to boot on single user mode (press and hold `Cmd + S + R ) and run the following commands.

```shell
fsck -fy # to check a disk
mount -uw / # mount a root filesystem with read/write permissions
sudo mkdir /AMD_Kexts/ # make a directory to store the AMD drivers in case you'll need them in future
sudo mv /System/Library/Extensions/AMD*.* /AMD_Kexts/ # move the AMD drivers
sudo rm -rf /System/Library/Caches/com.apple.kext.caches/ # remove the AMD drivers cache
sudo mkdir /System/Library/Caches/com.apple.kext.caches/ # just in case OS X will be dumb and will not recreate this directory, I am creating it for OS X
sudo touch /System/Library/Extensions/ # to update the timestamps so that new driver caches - without AMD drivers - will be definitely rebuilt
sudo umount / # umount a partition to guarantee that your changes are flushed to it
sudo reboot
```

However, in the same way as the solution's poster, when I tried to delete the kext the Mac was throwing me the error `operation not allowed` or something similar. Probably because in the same way as s/he, I have my disk locked as "read-only" after too many attempt of booting. Lucky, I didn't need to mount my disk on Linux, as s/he did. I just took my disk out of my Mac and put it in a USB enclosure that I connected to another Mac with High Sierra. I have High Sierra installed in my machine with the new APFS, so that means that my disk in only readable by other Macs with High Sierra installed. Dangerous, yes, but I wanted to take advantage of the new features. Backups were invented for some reason.

From there, I just needed to performed the same commands but a little bit different. You have to take into account that in macOS your hard drive is going to mount automatically, so it wasn't necessary to mount it like before, and you just have to run the rest of the commands with the proper path and the name of your drive. In my case my hard drive name is `Macintosh SSD`, and in this Mac there is also a `Macintosh SSD`, so when it mounted my hard drive macOS renamed it to to `Macintosh SSD 1`, In the shell you have to proper indecate the blank spaces on name and paths using the backslash symbol `\`, therefore I could access to my hard drive using the name `Macintosh\ SSD\ 1`. Mind the name of your hard drive (usually `Macintosh HD`) and change the path in the commands in consequence.

```shell
sudo mkdir /Volumes/Macintosh\ SSD\ 1/AMD_Kexts/ # make a directory to store the AMD drivers in case you'll need them in future
sudo mv /Volumes/Macintosh\ SSD\ 1/System/Library/Extensions/AMD*.* /AMD_Kexts/ # move the AMD drivers
sudo rm -rf /Volumes/Macintosh\ SSD\ 1/System/Library/Caches/com.apple.kext.caches/ # remove the AMD drivers cache
sudo mkdir /Volumes/Macintosh\ SSD\ 1/System/Library/Caches/com.apple.kext.caches/ # just in case OS X will be dumb and will not recreate this directory, I am creating it for OS X
sudo touch /Volumes/Macintosh\ SSD\ 1/System/Library/Extensions/ # to update the timestamps so that new driver caches - without AMD drivers - will be definitely rebuilt
sudo umount /Volumes/Macintosh\ SSD\ 1/ # umount a partition to guarantee that your changes are flushed to it<br>
```

Now, you can take your disk, reinstall it in your Mac and begin from there. I booted to something like this[^1]:

{% include figure image_path="assets/images/blog/2018/IMG_4422.jpg" alt="Booting without kexts." caption="Booting without kexts." %}

### Using solutions 2, 3 and 4

That's beginning… at least now I knew that my computer can be booted. Then, I decided to switch solutions and continue with the fix explained in [2,](https://forums.macrumors.com/threads/disable-a-failed-amd-gpu-on-a-2011-macbook-pro-grub-solution.2087527/) which is fully explained in [3](https://gist.github.com/blackgate/17ac402e35d2f7e0f1c9708db3dc7a44). The reason… because seemed more recent and better explained, and more stable in the long term. So, as it's detailed in that solutions, first reset the [SMC](https://support.apple.com/en-us/HT201295) and the [NVRAM](https://support.apple.com/en-us/HT204063). Then, boot your Mac on recovery single user mode (pressing and holding `Cmd + S + R`) and run the following commands.

```shell
nvram fa4ce28d-b62f-4c99-9cc3-6815686e30f9:gpu-power-prefs=%01%00%00%00
csrutil disable
reboot
```

Now, and since you moved the GPU kext from their original location you are going to boot to something like this.

{% include figure image_path="assets/images/blog/2018/IMG_4423.jpg"  alt="Booting normally" caption="Booting normally" %}

Hey!! you probably are thinking… I've done it, I've fixed. it. Yes & not. Since you've moved the kexts from their original place things are working more or less correctly, however, if you updated the system, you are going to probably have to move your kext and apply the solution again. For that reason, they decided to created a nicer solution implementing [GRUB](https://en.wikipedia.org/wiki/GNU_GRUB) on your start up disk and blocking the loading of those kexts on booting. GRUB is the same system you put in place when you are installing Linux in a computer and you want to have more than one OS in the machine. This solution allow us to keep the kexts in place and boot without loading them. The solution isn't perfect, but at least is easy to implement and in case you install a system update you can easily reimplement.

At this point, I recommend to move the kexts to the original location `/System/Library/Extensions/`.

{% include figure image_path="assets/images/blog/2018/Screen-Shot-2017-12-04-at-12.43.04.png" alt="Moved kexts" caption="Moved kexts" %}

  You can do it dragging and dropping those back to its original location (it's going to ask for your password), or you just can move then with terminal:

```shell
sudo mv /AMD_Kexts/*.* /System/Library/Extensions/
```

#### Getting a GRUB

Now, to implement the complete solution you have to [download ubuntu](https://www.ubuntu.com/download/desktop) to take the GRUB from there. I've downloaded [Ubuntu ~~17.10~~](https://www.ubuntu.com/download/desktop)[^3], as it's specified in the fix. When you've downloaded the .ISO, you have to attach and mount it, so assuming that you have the ISO in downloads:

```shell
hdiutil attach -nomount ~/Downloads/ubuntu-17.10-desktop-amd64.iso
```

Which probably turns something like this:

```shell
/dev/disk2              Apple_partition_scheme
/dev/disk2s1            Apple_partition_map
/dev/disk2s2            Apple_HFS
```

The disk number could be different, for example in my case the first time was disk3, but mounted a second time and was disk2. Depends on how many disks have you mounted before.

Now you can finally mount the ISO with the following commands:

```shell
mkdir /tmp/ubuntu
mount -t cd9660 /dev/disk2 /tmp/ubuntu/
open /tmp/ubuntu/shell
```

#### Preparing the USB stick and editing GRUB file

You have to [format you USB stick to FAT32](https://support.apple.com/kb/PH22241?locale=en_GB) and it's recomendable to name it `boot` folders from the ISO to your USB stick root.

{% include figure image_path="assets/images/blog/2018/Screen-Shot-2017-12-04-at-13.38.12.png" alt="Folders you have to copy" caption="Folders you have to copy" %}

When you have those folders on your USB stick, you have to edit the file `/RESCUE/boot/grub/grub.cfg`. I like Atom to edit, but perhaps you don't have it installed so if you type:

```shell
open /Volumes/RESCUE/boot/grub/grub.cfg
```

Your default text editor will open. In case you want to be sure and open with Text Edit:

```shell
open -a TextEdit /Volumes/RESCUE/boot/grub/grub.cfg
```

Then you have to delete all the file content and paste the following.

```shell
if loadfont /boot/grub/font.pf2 ; then
    set gfxmode=auto
    insmod efi_gop
    insmod efi_uga
    insmod gfxterm
    terminal_output gfxterm
fi

set menu_color_normal=white/black
set menu_color_highlight=black/light-gray

set timeout=0
menuentry "macOS" {
    outb 0x728 1
    outb 0x710 2
    outb 0x740 2
    outb 0x750 0
    outb 0x714 0xFF
    exit
}
```

Take into account that is a specific GRUB configuration for High Sierra and with a single OS installed. If you have more that one OS or you are under other OS check [this](https://gist.github.com/blackgate/17ac402e35d2f7e0f1c9708db3dc7a44#34-edit-the-grubcfg-file). Note that I've added a line to the original proposed GRUP file at almost at the end. This is because someone suggest that line on this [thread](https://forums.macrumors.com/threads/disable-a-failed-amd-gpu-on-a-2011-macbook-pro-grub-solution.2087527/page-4#post-25518184). I've also set the `timeout` variable to zero, since I don't want to push enter to continue or wait 10 seconds. I don't have anything to chose from.

Save the file and let's copy now to you Mac. Or perhaps, you can test if this works properly rebooting using the USB stick. For that you have to reboot and then push `alt` after you hear the chime, introduce the USB stick and choose it on the menu. You should be able to reboot normally.

#### Making it permanent

Now you can make this permanent and without need the USB. You run on terminal the following commands with your USB still plugged and assuming that you named it `RESCUE`.

```shell
cd /Volumes
sudo mkdir efi
sudo mount -t msdos /dev/disk0s1 /Volumes/efi
sudo mkdir /Volumes/efi/boot
sudo mkdir /Volumes/efi/EFI/grub
sudo cp -R /Volumes/RESCUE/boot/ /Volumes/efi/boot
sudo cp -R /Volumes/RESCUE/EFI/boot/ /Volumes/efi/EFI/grub
sudo bless --folder=/Volumes/efi --file=/Volumes/efi/EFI/grub/grubx64.efi --setBoot
sudo bless --mount=/Volumes/efi --file=/Volumes/efi/EFI/grub/grubx64.efi --setBoot
```

Now… you can unmount the USB and boot without it.

#### Preventing GPU from waking up from sleep

When you are under High Sierra, the next step doesn't really work, and even installing this kext is going to return to a black screen when you return form sleep. To prevent this, you can just change the way your Mac sleep and make it hibernate. If you have a SSD in place, like I have, the difference on time between waking up from a normal sleep than from hibernation is going to be neglectable.  The only real difference is, your computer isn't going to wake up when you lift the lid and you have to push the on/off button to wake your Mac up. To set this up you have to run on terminal:

```shell
sudo pmset -a hibernatemode 25
```

If you want to return to the normal sleep mode you set in the following way:

```shell
sudo pmset -a hibernatemode 3
```

Anyhow, I decided to apply the solution as it's explained [here](https://github.com/blackgate/AMDGPUWakeHandler) and I even [created a kext myself for High Sierra](/assets/docs/AMDGPUWakeHandler.kext.zip/). Just in hopes that in a close future things improve I can normally sleep. If you download the kext you just have to unzip it and then copy to `/Library/Extensions` and run the following commands.

```shell
sudo chmod -R 755 /Library/Extensions/AMDGPUWakeHandler.kext
sudo chown -R root:wheel /Library/Extensions/AMDGPUWakeHandler.kext
sudo touch /Library/Extensions
```

And reboot.

Now, you are done.

#### Reimplementing the fix

If you need to reimplement the solution because you updated the system you are going to need to just bless GRUB disk again —make it a bootable disk. You just boot from the rescue USB stick and run the following commands on terminal[^2]

```shell
cd /Volumes
sudo mkdir efi
sudo mount -t msdos /dev/disk0s1 /Volumes/efi
sudo bless --folder=/Volumes/efi --file=/Volumes/efi/EFI/grub/grubx64.efi --setBoot
sudo bless --mount=/Volumes/efi --file=/Volumes/efi/EFI/grub/grubx64.efi --setBoot
```

## Final note

As I've mentioned earlier, there are only two caveats to this solution if you are under High Sierra. One is that you lose the ability to sleep you computer normally and you have to hibernate. Nothing really serious if you have an SSD. The other is more serious… or at least is going to affect you more in your daily basics. You are not going to be able to control the brightness of your computer anymore. It's something that [MacRumors community](https://forums.macrumors.com/threads/disable-a-failed-amd-gpu-on-a-2011-macbook-pro-grub-solution.2087527/page-5) is trying to fix.

Anyhow, the weir way that High Sierra manages the bright isn't something new. And during the betas, some people commented about this on the developers forum as well, [here](https://forums.developer.apple.com/thread/80705) and [here](https://forums.developer.apple.com/thread/82500).

### Why this happens?

You're probably wondering yourself why this is happening to your computer although if you have researched a little bit about the problem you probably already know what is the problem. Really bad quality graphic chips that due to heat in the computer finally fails. You can learn a little bit in this video.

{% include video id="1AcEt073Uds" provider="youtube" %}

To sum it up… AMD / Nvidia chips mounted in logic board of this model are really bad and I guess most of them are faulty. The lack of proper ventilation on Macs, specially in this models, doesn't improve things and make then even more prone to this kind of issues. There is a technique called reballing, which basically consist repair the littles balls that the chip uses to connect to the logic board. However, this is just a temporal solution since the problem is in the chip itself. Reballing works just because when you heat up the chip to reball it you mess with some of the internal parts of the chip —I think they call this [reflow](https://en.wikipedia.org/wiki/Reflow_soldering)— and that makes it to work properly again, but after a while the problem is going to return. I've read that some people just put the board in the oven at around 200ºc to fix the issue, but I guess doesn't last a lot either.

The only long working solution is just get another board, or just another chip, and pray that this time it isn't faulty. Change the board isn't complicated, at least not incredible. But invest 400€, or even more, in a 6 years old computer is something you really have to ponder.

Anyway, the only ones here to blame is AMD / Nvidia and or course Apple, they knew the issue all this time and they've done nothing to fix it. And for that reason [Apple is being sued](https://www.macworld.co.uk/news/mac/macbook-pro-graphics-failures-addressed-by-apple-repair-programme-3497935/).

It's time to think if it's really worth to spend the money Apple tags their computers. I'm really willing pay, but I expect a top-notch silent computer and service.  Besides, it seems that Apple has forgotten that professional customers really need professional computers and the last batch seems that [isn't up to the mark](https://www.theverge.com/2016/11/7/13548052/the-macbook-pro-lie) and Apple has received a lot of backlash for it. So big has been the adverser reaction that Apple has to even make an [statement](https://www.macworld.co.uk/news/mac/new-macbook-pro-2018-rumours-uk-release-date-price-features-specs-3661715/) addressing it.

### Related issues?

While I was in the US we got a beautiful 24" cinema display that I connected to my machine and we enjoyed a lot. At that moment, I was concerned by the _extensive_ use I was doing of the dGPU when connected to the external screen and I asked in the Apple communities about the issue. They replied that _[it was the intended way to use the computer and I shouldn't worry](https://discussions.apple.com/thread/7963970)_. I don't blame then, because what they told me, was true, it was/is the normal way to use the computer and how it was designed.  Perhaps bad designed, or at least not really thoroughly thought. When you connect the computer to a external screen you trigger the use of the dGPU, not because you really need it —you can perfectly work with the integrated as other models do, but because the thunderbolt connection where you connect the external screen leads directly to the graphic card there is no way to avoid it's use. This cause an increase of temperature of more or less 20º from the normal operation, just for connecting a external display, and  in my humble opinion there is nothing of _extensive_ use in connecting it to a external display.

In the same way… An almost month ago I installed High Sierra and I noticed a weird error related to graphics on the console:

{% include figure image_path="assets/images/blog/2018/Screen-Shot-2017-12-04-at-00.23.58.png" alt="[ERROR] — Unknown CGXDisplayDevice: 0x41dcd00" caption="[ERROR] — Unknown CGXDisplayDevice: 0x41dcd00" %}

The error is still there right now, and I told Apple about it [when I noticed](https://twitter.com/lpuerto/status/928885729916342272). They contacted me and collected some data. They never return to me about this issue. Now, I wondering if this two issues are connected and High Sierra accelerated the degradation process of the Nvidia / AMD chip.

[^1]: To be entirely honest I don't remember if after deleting the kexts I was able to boot directly to the striped desktop or I needed to run the commands in the beginning of solution [2](#using-solutions-2-3-and-4).
[^2]: Today, 8th of December 2017, I decided to install the update to 10.13.2. It worked really well, and even booted without needed to apply the fix again. In other words, the discrete GPU was working. However, after a while fail, and took me more than what I wanted to reestablish everything. So my recommendation is, if you update, apply the fix as soon as possible, but cause sooner or later things are going to go south and it's going to take you even more time to fix it. In my case I needed to apply the solution almost from the very beginning and a NVRAM reset was necessary to be able to operate again the computer. Good luck!
[^3]: Previusly I downloaded a spefific release of Ubuntu. I've changed to the last one. 
