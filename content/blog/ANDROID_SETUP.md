---
title: Setup android the nerdy way
date: 2024-11-16
index: 20
draft: true
---

With time passing by i'm getting more and more addicted to the terminal workflows that i've built, so Android devices for me are such a pain to work with but from time to time my main laptop runs out of charge so i'm forced to work with my tablet.

 Without my terminal setup i feel lost so i wrote this guide down to guide me in the configuration of a [termux](https://termux.dev/en/) environment that i can work with, not as good as a real linux machine but i guess it's ok for a backup device

- install `wireguard` and `termux` from the app store
- install the following application from `termux` package manager:

```bash
pkg install vim git make fzf tmux lazygit jq
```

- mount the machine filesystem to the termux directories

```bash
termux-setup-storage
```

- clone vim configs and install them

```bin
git clone https://github.com/carnivuth/toolbox
cd toolbox
./toolbox.sh
```

And it "*should*" be working, i hope
