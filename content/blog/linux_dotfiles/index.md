---
title: Dotfiles, how i started my linux adventure
description: "My personal linux experience with dotfiles, system administration, and distro hopping"
date: 2024-11-16
aliases: []
tags:
    - shell
    - window managers
    - dotfiles
    - git
    - github
    - linux
    - arch
    - stow
    - vim

draft: false
---

My personal linux journey started when i was in high school and teachers started to show us how production environments where managed. I still remember to this days some of my professor advises:

> before doing anything update the system repositories by typing `sudo apt update`

## HOW IT STARTED

At the time i wasn't daily driving a linux box in all of my machines as i do today, to me linux seemed as a server only solution, even if i recognize some big advantages for example update the all system with a single command.

At university i started working more on linux systems (*even take a course focused on system administration*), i also start to develop web applications and at work linux was a more handy system as testing the software in a production like environment was easier.

So i started with the go to option for beginners: `ubuntu`, at the time was at version `20.04`, and the software suite i was using at the time was:

- `phpstorm` and `vscode` as editors mainly for PHP development
- `vagrant` and `virtualbox` for testing
- docker for university systems

## UBUNTU LIMITS

But i quickly started to notice the distribution limits: bloatware deb packages installing snap ones, some application need to be installed downloading the deb package from the website and so on, but the most annoying limit was **window management** as the GNOME DE that ubuntu comes preinstalled with as no simple way to manage windows by tiling them.

For some times i tried the [gnome extension from system 76](https://github.com/pop-os/shell) that implements tiling windows, it was a little junky with some applications (*especially those that uses qt*) but it was working fine until one day that i decided to update the system packages and a new version of gnome broke the compatibility with the plugin 😢

So i decided to migrate from ubuntu to a more poweruser friendly option, [arch linux](https://archlinux.org/) 💪.

## WHY I FELL IN LOVE WITH ARCH LINUX

Why arch? cause there was no bloatware preinstalled and i was free to experiment with window managers and i quickly fall in love with the distribution ideals, from the [arch wiki](https://wiki.archlinux.org/title/Arch_Linux#User_centrality)

> Whereas many GNU/Linux distributions attempt to be more user-friendly, Arch Linux has always been, and shall always remain user-centric. The distribution is intended to fill the needs of those contributing to it, rather than trying to appeal to as many users as possible. It is targeted at the proficient GNU/Linux user, or anyone with a do-it-yourself attitude who is willing to read the documentation, and solve their own problems.

So i started daily drive arch with i3 window manager and dotfiles start to be a big part of my system management operations, i also started to write some small scripts to automate some simple operations (*backup data,writing notes ecc...*).

The next step was obvious, if i need to write text files of any kind then `git init` must be executed and i started to use `stow` for linking files in the correct directories.

## THE RESULTS

Then i started to save all dotfiles and scripts in a  [github repository](https://github.com/carnivuth/scripts), today i use [hyprland](https://hyprland.org/) as my window manager and i have migrated to a terminal based workflow that revolves around `vim` and other terminal only software to be more efficient, This is by far the project that took the most time to create and maintain  😅

I leave here an installation guide for a freshly arch box, the idea is:

{{< mermaid >}}
flowchart LR
A[insert USB stick]
B[wget this page]
C["`**GREP AND PASTE LIKE THERE IS NO TOMORROW**`"]
A --> B --> C
{{< /mermaid >}}

## INSTALLATION GUIDE

- Download latest arch  ISO from the [official website](https://archlinux.org/download/)

- write the image to a USB pen

```bash
dd if=/Downloads/archlinux.iso of=/dev/<usb pen> status=progress
```

### INSIDE LIVE ENVIRONMENT

- connect to network

```bash
iwctl station wlan0 connect <BSSID>
```

- setup `pacman` parallel downloads and update local db info

```bash
pacman -Sy
pacman -S sed
sed -i "s/#ParallelDownloads.*/ParallelDownloads = $(nproc)/g" '/etc/pacman.conf'
```

- install git and vim

```bash
pacman -S git vim
```

- run the official installation script, here i leave a sample of the [configuration](arch/user_configuration.json) file

```bash
curl https://blog.carnivuth.org/blog/linux_dotfiles/arch/user_configuration.json > user_configuration.json
archinstall --config user_configuration.json
```

### INSIDE THE INSTALLED SYSTEM

- configure `greetd`

```bash
pacman -S sed
sed -i 's|command.*/command = "agretty --cmd /bin/bash"/g' '/etc/greetd/config.toml'
```

- configure `sudo`

```bash
echo "$USER ALL=(ALL:ALL) NOPASSWD:/bin/pacman" > "/etc/sudoers.d/$USER"
```

- download the dotfiles repos (*yes there is 2 of them 😅*)

```bash
# scripts
cd $HOME && \
git clone https://github.com/carnivuth/scripts && \
cd scripts && ./scripts.sh

# toolbox
cd $HOME && \
git clone https://github.com/carnivuth/toolbox && \
cd scripts && ./toolbox.sh
```
