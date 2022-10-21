---
title: Disconnecting the dGPU in a late 2011 MacBook Pro —third way
date: 2017-12-11 15:03:00
last_modified_at: 2019-10-20 19:24
header:
  overlay_image: assets/images/blog/2018/grey_screen.jpg
  teaser: assets/images/blog/2018/grey_screen.jpg
categories:
  - Personal
  - Technology
tags:
  - dGPU
  - graphic card
  - high sierra
  - how to
  - macOS
---
**Update 2017.12.12-14.20 EET**: whether or not I use the [AMDGPUWakeHandler](https://github.com/blackgate/AMDGPUWakeHandler) and whether I sleep or hibernate I can't wake up of the hibernation/sleep. Depending on if I have [AMDGPUWakeHandler](https://github.com/blackgate/AMDGPUWakeHandler) on or off I get different outputs, but none of those end in a successful wakeup.

**Update 2017.12.13-09.08 EET**: After checking about the wake up problem after sleeping / hibernating with the people form MacRumors, I reached some conclusions.

  1. You don't use [AMDGPUWakeHandler](https://github.com/blackgate/AMDGPUWakeHandler) with this solution, since it could create a kernel panic… so for that reason doesn't work.
  2. `pmset gpuswitch` option can help, but it's really undocumented officially, so we really don't know what the values for 0, 1 and 2 stand for, and can vary from machine to machine I guess, or at least to macOS version to version.
  3. gfxCardStatus can help since the problem after wake up is the dGPU activates and the computer freezes.

I've updated steps 11 and 12 in consequence.

**Update 2017.12.18-14.42 EET**: I've tried to wake up from hibernation without gxfCardStatus and it worked pretty well I didn't have any issue, so if you don't want to have it installed or at least running in the background I think it's OK.

**Update 2018.01.09-21.35 EET**: After install the [security update to mitigate the effects of Spectre](https://support.apple.com/en-gb/HT208397), I have to apply the fix again as explained [here](/blog/2017/12/11/disconnecting-the-dgpu-in-a-late-2011-macbook-pro-third-way/#in-case-you-have-to-update). Everything worked fine, but on wake up of hibernation I got a black screen a couple of times. Also the computer didn't turn off and got stuck in a black screen. I really don't know what is the reason, but seems it's related to the `gpuswitch` parameter. I changed to 0 and then to 2 again, and seems that everything is normal again. But I don't know if it's really that or it's other thing.

**Update 2018.01.21-09.20 EET**: I just installed macOS update 10.13.3 and after testing a little bit I got to the conclusion that what work best for me is to set `pmset -a gpuswitch 1`

**Update 2019.10.20-22.10 EEST**: I don't have this computer anymore, so I'm sorry but probably I'm not going to be able to help you. 

* * *

Ok!!!!!! There is a third, and I think final, solution to totally deactivate the dGPU. Till this moment this is my favorite solution and I even have the brightness back to my computer. Also it sleeps correctly. You can check my previous post also [1](/blog/2017/12/05/my-macbook-pro-late-2011s-discrete-graphics-card-said-ciao-again/) & [2](/blog/2017/12/08/disconnecting-the-dgpu-in-a-late-2011-macbook-pro-another-way/).

We all have to thank to [MacRumors community](https://forums.macrumors.com/threads/force-2011-macbook-pro-8-2-with-failed-amd-gpu-to-always-use-intel-integrated-gpu-efi-variable-fix.2037591/page-50#post-25581450) that all of them have been working really hard to create a workable solution to all of us. This guide is almost a exact copy of the one posted by MikeyN [here](https://forums.macrumors.com/threads/force-2011-macbook-pro-8-2-with-failed-amd-gpu-to-always-use-intel-integrated-gpu-efi-variable-fix.2037591/page-35#post-24956091). I've just changed somethings and added the [AMDGPUWakeHandler](https://github.com/blackgate/AMDGPUWakeHandler) to manage the sleep.

## The fix

Let's explain how it's done:

1. As always you have to reset [SMC](https://support.apple.com/en-us/HT201295) and [PRAM/NVRAM](https://support.apple.com/en-us/HT204063) before you do anything else.
   * **SMC**: shutdown, unplug everything except power, now hold `leftShift + Ctrl + Opt/Alt + Power` for about 10" and release at the same time.
   * **PRAM/NVRAM**: with the power cord on, power on and immediately later and before the chime hold `cmd + Opt/Alt + P + R` at the same time until you hear the chime for the second time. Try to do the following step just right after, so you don't let the computer to load —and fail.

2. Now, boot into recovery single user mode by holding: `cmd + R + S`. When finish to load, you run:

   ```shell
   $ csrutil disable # to disable SIP
   $ nvram fa4ce28d-b62f-4c99-9cc3-6815686e30f9:gpu-power-prefs=%01%00%00%00 # to disable the dGPU on boot.
   $ nvram boot-args="-v" # Load in verbose mode
   $ reboot
   ```

3. Reboot in single user mode holding on boot `cmd + S`

4. Now we are going to mount the hard drive and move the driver of the dGPU `AMDRadeonX3000.kext` out of the drivers folder.

   ```shell
   $ /sbin/mount -uw / # mount root partition writeable
   $ mkdir -p /System/Library/Extensions-off # make a kext-backup directory
   $ mv /System/Library/Extensions/AMDRadeonX3000.kext /System/Library/Extensions-off/ # only move ONE offending kext out of the way
   $ touch /System/Library/Extensions/ # let the system update its kextcache
   $ reboot
   ```

5. Now you're going to be able to load your desktop normally, but with an accelerated iGPU display. However, the system doesn't know how to power-management the failed AMD-chip, so you are going to need to load it manually.

   ```shell
   $ sudo kextload /System/Library/Extensions-off/AMDRadeonX3000.kext
   ```

6. You can automate the loading with the doing the following:

   ```shell
   $ sudo mkdir -p /Library/LoginHook # Creating a folder to store the script.
   $ sudo nano /Library/LoginHook/LoadX3000.sh # Creating the script.
   ```

7. On nano you type/paste:

   ```sh
   #!/bin/bash
   kextload /System/Library/Extensions-off/AMDRadeonX3000.kext
   # pmset -a gpuswitch 0 # to prevent to switch to the dGPU
   exit 0
   ```
   \* I've decided to comment the line 3 since I'm not sure that 0 is the correct value. Besides, in the step 12 I set `gpuswitch 2`.

8. You make it executable active:

   ```shell
   $ sudo chmod a+x /Library/LoginHook/LoadX3000.sh
   $ sudo defaults write com.apple.loginwindow LoginHook /Library/LoginHook/LoadX3000.sh
   ```

9. This is what I like the most. You create a script in the root of your hard drive to automate the process in case of an update.

   ```shell
   $ sudo nano /force-iGPU-boot.sh # Creates the script in the root
   ```
   With the following content.

   ```sh
   #/bin/sh
   sudo nvram boot-args="-v"
   sudo nvram fa4ce28d-b62f-4c99-9cc3-6815686e30f9:gpu-power-prefs=%01%00%00%00
   exit 0
   ```

10. Now you make it executable and I hide to avoid delete it:

    ```shell
    $ sudo chmod a+x /force-iGPU-boot.sh # make ir executable
    $ sudo chflags hidden /force-iGPU-boot.sh # hide the file.
    ```
    In the future if you reset SMC and PRAM/NVRAM you just have to load in single user mode holding `cmd + S` and run:

    ```shell
    $ sh /force-iGPU-boot.sh
    ```

11. ~~Then, you can copy the [AMDGPUWakeHandler](https://github.com/blackgate/AMDGPUWakeHandler), or the one I created A[MDGPUWakeHandler.kext](/assets/docs/AMDGPUWakeHandler.kext.zip/), to `/Library/Extensions` and run the following commands:~~
    ```shell
    $ # sudo chmod -R 755 /Library/Extensions/AMDGPUWakeHandler.kext
    $ # sudo chown -R root:wheel /Library/Extensions/AMDGPUWakeHandler.kext
    $ # sudo touch /Library/Extensions
    ```

12. ~~This time you don't need to change the way the machine sleeps. And you can reboot~~

     I recommend you the way the machine sleeps to hibernate, but I haven't tested if it can sleeps normally after we apply the following step

    ```shell
    $ sudo pmset -a hibernatemode 25
    ```

    All the fuss about the wake up after sleep / hibernate is related to when the computer wake ups checks the GPUs and somehow it gets stuck to dGPU. For that reason some people has changed the variable gpuswitch in pmset. Nevertheless, this variable is really undocumented and you find explanations to what the values to that variable (0, 1 and 2) do on internet. ~~At this moment I have it set as default 2~~ I've decided to change to 1.

    ```shell
    $ sudo pmset -a gpuswitch 1
    ```

    ~~Which I thing it's the default value.~~ The default value is 2.

    You can try the different values, reboot and then close the lid and wake up and see the results.

    What has worked for me is leave it in `1` ~~and install [gfxCardStatus](https://gfx.io), and every time I boot change to `integrated only`. But still testing. I've tried to wake up from hibernation without gfxCardStatus and it also worked, so I guess it's optional.~~ [gfxCardStatus](https://gfx.io) can help, but at least I'm not using it right now. I wake up from hibernation without it perfectly.

13. After you reboot and if everything goes smoothly, perhaps you wan to return to the normal boot mode. You can room in terminal:

    ```shell
    $ sudo nvram boot-args=""
    ```

Now I just going to copy paste from [MikeyN's post](https://forums.macrumors.com/threads/force-2011-macbook-pro-8-2-with-failed-amd-gpu-to-always-use-intel-integrated-gpu-efi-variable-fix.2037591/page-35#post-24956091)

>This setup has now one kext in a place Apple's installers do not expect. That is why in this guide <span style="background-color: #ffff00;">SIP has not been reenabled. If an update that contains changes to the AMD drivers is about to take place it is advisable to move back the AMDRadeonX3000.kext to its default location before the update process. Otherwise the updater writes at least another kext of a different version to its default location or at worst you end up with an undefined state of partially non-matching drivers.
>
><span style="background-color: #ffff00;">After any system update the folder /System/Library/Extensions has to be checked for the offending kext.</span> Its presence there will lead to e.g. a boot hang on Yosemite and Sierra, an overheating boot-loop in High Sierra.
>
>Further: this laptop is overheating, no matter what you do. The cooling system is inadequate and the huge number of failing AMD chips are just proof of that.

## In case you have to update

So, before you update the system, please remember to run:

```shell
sudo cp -r /System/Library/Extensions-off/AMDRadeonX3000.kext /System/Library/Extensions/
```

Then you update. If you can't normally load your computer, you can hold `cmd + S` and run:

```shell
sh /force-iGPU-boot.sh
```

Then you can reboot again on single user mode holding `cmd + S` and then run

```shell
sudo mv /System/Library/Extensions/AMDRadeonX3000.kext /System/Library/Extensions-off/
```

To move again the kext. Keep in mid that the other one still there do you are going to probably rename it in this fashion:

```shell
sudo mv /System/Library/Extensions/AMDRadeonX3000.kext /System/Library/Extensions-off/AMDRadeonX3000-1.kext
```

## Checking that everything is OK

If you run in terminal

```shell
kextstat | grep AMD
```

You have to get something similar to this:

```shell
111    2 0xffffff7f82da8000 0x122000   0x122000   com.apple.kext.AMDLegacySupport (1.6.0) 3BE3756A-6D69-3CD0-B18A-BC844EE2A4DF <105 12 11 7 5 4 3 1>
130    0 0xffffff7f83631000 0x12e000   0x12e000   com.apple.kext.AMD6000Controller (1.6.0) DC45A18B-6F81-38D5-85CB-06BFBD74B524 <111 105 12 11 5 4 3 1>
146    0 0xffffff7f83126000 0x22000    0x22000    com.apple.kext.AMDLegacyFramebuffer (1.6.0) 5F948DD4-8D1E-31BD-A7EE-C44254CBA506 <111 105 12 11 7 5 4 3 1>
174    0 0xffffff7f83ab3000 0x56c000   0x56c000   com.apple.kext.AMDRadeonX3000 (1.6.0) 7E721EBE-AD4B-3C53-A70A-1FFF3C231968 <173 147 105 12 7 5 4 3 1>
```

In the beginning I wasn't getting any of these and the system just loaded AMDRadeonX3000. That resulting in a little bit of overheating in the dGPU and I wasn't able to sleep the computer. The reason for the system to not load the kext was I moved them that much that I changed the ownership of the files. If that is your case you can run in terminal:

```shell
sudo chown -R root:wheel /System/Library/Extensions/AMD*.*
sudo chown -R root:wheel /System/Library/Extensions-off/AMD*.*
```

to return the ownership to the System / Root

Let's hope that everything goes smoothly from now on. You have to see the bright side of life, now you have a quite cold running Mac since the dGPU is totally deactivated. After a while your temps have to be something similar to this:

{% include figure image_path="assets/images/blog/2018/Screen-Shot-2017-12-11-at-14.46.20.png" alt="GPU diode and GPU proximity are the dGPU sensors. GPU PECI is the iGPU sensor." caption="GPU diode and GPU proximity are the dGPU sensors. GPU PECI is the iGPU sensor." %}{: .align-center}
