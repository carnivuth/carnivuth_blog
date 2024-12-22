---
title: How to gpg
date: 2024-12-09
id: EXPORT_GPG_KEYS
aliases: []
tags: []
draft: true
---

Just the oneliner, export_gpg_keys.md should be self explanatory

```bash
gpg --output backupkeys.pgp --armor --export-secret-keys --export-options export-backup user@email
```
