---
title: Windows personal customization guide
date: 2024-11-16
index: 21
draft: true
---


- create a bootable pen with [ventoy](https://www.ventoy.net/en/index.html)

- copy the image on the ventoy image partition

```bash
sudo mount /dev/sda1 /mnt
sudo cp ~/Download/win11.iso /mnt
```

- mount pen and install windows

- install winget from the [microsoft store](https://apps.microsoft.com/detail/9nblggh4nns1?hl=en-us&gl=IT)

- install git

```powershell
winget install Git.Git
```

- install applications with `winget`

```bash
echo '{"$schema":"https://aka.ms/winget-packages.schema.2.0.json","CreationDate":"2024-06-18T21:47:02.054-00:00","Sources":[{"Packages":[{"PackageIdentifier":"Discord.Discord"},{"PackageIdentifier":"Git.Git"},{"PackageIdentifier":"Microsoft.Edge"},{"PackageIdentifier":"Microsoft.EdgeWebView2Runtime"},{"PackageIdentifier":"Microsoft.AppInstaller"},{"PackageIdentifier":"Microsoft.UI.Xaml.2.7"},{"PackageIdentifier":"Microsoft.UI.Xaml.2.8"},{"PackageIdentifier":"Microsoft.VCLibs.Desktop.14"},{"PackageIdentifier":"Microsoft.DevHome"},{"PackageIdentifier":"Microsoft.WindowsTerminal"},{"PackageIdentifier":"Mozilla.Firefox"},{"PackageIdentifier":"Mozilla.Thunderbird"},{"PackageIdentifier":"Microsoft.OneDrive"},{"PackageIdentifier":"Spotify.Spotify"},{"PackageIdentifier":"Valve.Steam"},{"PackageIdentifier":"Ubisoft.Connect"},{"PackageIdentifier":"vim.vim"},{"PackageIdentifier":"Obsidian.Obsidian"},{"PackageIdentifier":"GitHub.cli"},{"PackageIdentifier":"Microsoft.VCRedist.2015+.x86"},{"PackageIdentifier":"Telegram.TelegramDesktop"},{"PackageIdentifier":"Nvidia.GeForceExperience"},{"PackageIdentifier":"Nvidia.PhysX"},{"PackageIdentifier":"Nextcloud.NextcloudDesktop"},{"PackageIdentifier":"Microsoft.PowerShell"},{"PackageIdentifier":"Microsoft.VCRedist.2012.x64"},{"PackageIdentifier":"Microsoft.VCRedist.2015+.x64"}],"SourceDetails":{"Argument":"https://cdn.winget.microsoft.com/cache","Identifier":"Microsoft.Winget.Source_8wekyb3d8bbwe","Name":"winget","Type":"Microsoft.PreIndexed.Package"}}],"WinGetVersion":"1.7.11261"}
' > programs.txt
winget import -i programs.json
```
