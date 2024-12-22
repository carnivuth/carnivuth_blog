---
title: Arch linux dotfiles installation
date: 2024-11-16
id: ARCH_SETUP
aliases: []
tags: []
draft: true
---

over the years i have customized a lot my arch linux installation, and pushed by the fear of loosing all my customization i've decided to manage all of my dotfiles in a [github repo](https://github.com/carnivuth/scripts).

Why i wrote this page? because i am lazy basically, and the idea of re-imagine the steps to reconfigure a new machine (*or this machine if for some reason i need to reconfigure it*) it's too much on my mental stack and prevents me from even start the process, so the idea is:

{{< mermaid >}}
flowchart LR
A[insert USB stick]
B[clone this repo]
C["`**GREP AND PASTE LIKE THERE IS NO TOMORROW**`"]
A --> B --> C
{{< /mermaid >}}

## STEPS

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

- run the official installation script

```bash
echo '{"additional-repositories":["multilib"],"archinstall-language":"English","audio_config":{"audio":"pipewire"},"bootloader":"Systemd-boot","config_version":"2.8.1","debug":false,"disk_config":{"config_type":"default_layout","device_modifications":[{"device":"/dev/nvme0n1","partitions":[{"btrfs":[],"dev_path":null,"flags":["Boot","ESP"],"fs_type":"fat32","mount_options":[],"mountpoint":"/boot","obj_id":"b465e03b-ca35-4c0e-a8db-a2d210749b92","size":{"sector_size":{"unit":"B","value":512},"unit":"GiB","value":1},"start":{"sector_size":{"unit":"B","value":512},"unit":"MiB","value":1},"status":"create","type":"primary"},{"btrfs":[{"mountpoint":"/","name":"@"},{"mountpoint":"/home","name":"@home"},{"mountpoint":"/var/log","name":"@log"},{"mountpoint":"/var/cache/pacman/pkg","name":"@pkg"},{"mountpoint":"/.snapshots","name":"@.snapshots"}],"dev_path":null,"flags":[],"fs_type":"btrfs","mount_options":["compress=zstd"],"mountpoint":null,"obj_id":"638e2947-6af9-4799-b713-0a092f6a5a38","size":{"sector_size":{"unit":"B","value":512},"unit":"B","value":254984675328},"start":{"sector_size":{"unit":"B","value":512},"unit":"B","value":1074790400},"status":"create","type":"primary"}],"wipe":true}]},"disk_encryption":null,"hostname":"","kernels":["linux"],"locale_config":{"kb_layout":"us","sys_enc":"UTF-8","sys_lang":"en_US"},"mirror_config":{"custom_mirrors":[],"mirror_regions":{"Italy":["https://it.mirrors.cicku.me/archlinux/$repo/os/$arch","https://archlinux.mirror.server24.net/$repo/os/$arch","http://it.mirrors.cicku.me/archlinux/$repo/os/$arch","http://archlinux.mirror.server24.net/$repo/os/$arch","http://archlinux.mirror.garr.it/archlinux/$repo/os/$arch"]}},"network_config":{"type":"nm"},"no_pkg_lookups":false,"ntp":true,"offline":false,"packages":["vim","git"],"paralleldownloads":0,"profile_config":{"gfx_driver":null,"greeter":null,"profile":{"custom_settings":{},"details":[],"main":"Minimal"}},"script":"guided","silent":false,"skip_ntp":false,"skip_version_check":false,"swap":true,"timezone":"Europe/Rome","uki":false,"version":"2.8.1"}' > user_configuration.json
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

- authenticate to `github`

```bash
gh auth login
```
