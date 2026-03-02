---
title: "The Linux Experience"
---

# Things I learned while using Ubuntu as a Desktop

# Installation
My installation went overall smoothly, but I did hit a few hurdles and learned some important information for future installs.

## Making a bootable flash drive
Most people will swear by software like Balena Etcher, but it turns out you can flash any `.iso` file to a usb on Mac and Linux without any additional software with the `dd` command.

I tried this a little after booting up ubuntu to try Arch on some ancient macs (which mostly failed /:). The [Arch Linux wiki](https://wiki.archlinux.org/title/Dd) (which has great tutorials on most linux things) has a guide on using this utility, but the way I used it was something along the lines of:
`sudo dd if='path/to/my/download.iso' of='' bs=4M status=progress`
Where the paths given to `if` and `of` are the file you want to copy the `Ubuntu.iso` in my case, and the drive location which would probably be something on `/dev` (use at your own risk!)

But, If you don't want to deal with those shenanigans something like Balena Etcher is just fine.

## The weirdness of UEFI and windows
I have heard of dreaded scenerios in which someone tries to dual boot windows and linux on the same machine, and linux end up getting disabled or worse. To my relief, this seems to be [only if you install linux first](https://askubuntu.com/questions/313623/can-i-install-ubuntu-first-before-installing-windows-for-dual-boot)

With that problem out of the way, I plugged in my bootable usb and tried and failed many times to install Ubuntu. Until I finally succeded after overcoming these problems:
1. Safe graphics mode: I don't know if this was actually a problem or not (see my second realization) but I kept getting a dumb error message while attempting to run the Ubuntu ISO. There are two options: one is `Try or install Ubuntu` and the other is `Ubuntu (safe graphics)` (note: these are names from memory and might not match the *exact* options). Safe graphics seemed to be the right option in the end with my NVIDIA GPU (I can hear the screams of older linux users)
2. There are two partitions on the USB that I flashed, one is just called `Ubuntu`, and the other is something like `UEFI ... Ubuntu` I didn't know what the UEFI one was and thought it might cause problems, so I just booted ubuntu. **wrong choice.** This made it seem like it was installing the right software, but no. Installing from this mode made my SSD incompatible with the UEFI (no, not in a permanent way), after *many* attempts, I made the stupid realization that I should try UEFI mode. After doing that it worked.
3. My wifi Card isn't supported by the installer Kernel ...or the installed Kernel. I am yet to figure this one out, but I would strongly advise anyone installing to **have an ethernet cable handy, and try all ethernet ports on your machine if you have multiple!**

After overcoming these two hurdles, I progressed to the next phase:
# Set up
> OhHhHhHHHH, BOooYyyY!

## Realizing that even ubuntu has challenges
Before installing, I was leaning to install Arch Linux (mostly for the meme and clout). I was afraid that there would be a painful lack of customizability and I would be stuck with closed source software (*\*gasp\**). But it turns out that Ubuntu is not lacking in challenge or adaptability.

## Dare to be stupid
> Don't

Luckily I don't have this problem, but I used to. DON'T BLINDLY FOLLOW AND RUN COMMANDS FROM **ANY** SOURCE. That means, online guides, forums, your favorite (or least favorite) LLM. Any of these could break your machine or hack you. Always `man` your command and see what it's trying to do especially if it is being `sudo`'d

## Some dubious advice
I got this advice mostly from ChatGPT, so take it with a grain of salt. But I have been encountering various performance problems while using ubuntu (including bluetooth being *super* unstable), and it claims that power saving mode is doing this. This checks out in my mind and seems to slightly improve some of these issues. So, be careful with the cursed button.

# Security
**DISCLAIMER: I am not a security professional, follow this guide at your own risk**
"Linux is soooooooo secure", they said, "Everything is already set up", they said, NO!
It turns out, that desktop linux has a serious lack of preconfigured safety. And while I can't and won't cover everything, these were the most egregious errors.
## Lack of a (configured) Firewall
Yep, anything on your network can send *whatever* it wants to your computer, **no questions asked.** This is usually considered **a serious problem**. Luckily (and unluckily) Ubuntu comes with an Easy to use firewall called `ufw`. What's the catch? IT'S NOT CONFIGURED OR EVEN TURNED ON AaAAAAaaaaAAaaAAaAAA. Anyways, here's how you do that:
- `sudo ufw allow ssh # allow ssh traffic`
- `sudo ufw default deny incomming`
- `sudo ufw default allow outgoing`
- `sudo ufw enable` (turn on the dang firewall)
If a network service just broke that normally worked you can
- `sudo ufw allow # YOUR PORT OR SERVICE NAME HERE #`
- (running `sudo ss -tupln` usually shows expected services)
And to see what is currently running:
- `sudo ufw status`

## Not many things are sandboxed
Any app you install, can, by default access any data on your drive that you can read and/or modify. This is ***bad***. If possible source your apps from snap or use AppArmor. I am still learning this too, so find yourself a more comprehensive guide.



# Useful guides and resources
I am not *completely* new to Linux, and thought that these resources and few tips would help a little:
- [Ask Ubuntu](https://askubuntu.com/) Is a forum for general Linux help. Beware the comment that appears on every post telling you that the [question is "stupid"](https://i.programmerhumor.io/2022/04/programmerhumor-io-stackoverflow-memes-programming-memes-7c1e2f95e444465.png)
- [Arch Linux Wiki](https://wiki.archlinux.org), though we are using ubuntu, this will have comprehensive guides and explanations of linux topics.
- [google](https://www.google.com) or your favorite search engine is almost always going to yield better results than any LLM.
