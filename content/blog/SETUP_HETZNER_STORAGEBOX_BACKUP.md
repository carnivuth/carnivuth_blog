---
title: setup backup with hetzner storagebox
date: 2024-09-29
draft: true
---

Hetzner is a cloud provider that offers various services, Paas and Saas and also cloud storage, in the form of storageboxes, in order to configure a backup for a new host

- create a storagebox subaccount to limit the user privileges of the backupped guest only to it's own files

```bash
ssh -l u425518  u425518.your-storagebox.de -p 23 mkdir -p <FOLDER>/.ssh
```

- create ssh keys for the storage box

```bash
ssh-keygen -f ~/.ssh/backup_key
```

- copy pubkey to remote

```bash
ssh -l u425518  u425518.your-storagebox.de -p 23 mkdir -p <FOLDER>/.ssh
scp -P 23 ~/.ssh/backup_key.pub u425518@u425518.your-storagebox.de:<FOLDER>/.ssh/authorized_keys
```

- setup ssh config

```bash
Host storagebox-address
    User subuser
    IdentityFile ~/.ssh/id_rsa
    Port 23
```

- setup sync script with remote (*for example `/usr/local/bin/sync_remote.sh`*)

```bash
#!/bin/bash
DEPS="rsync"
DATASTORES="" # PATHS to folders to sync with remote
REMOTE="" # REMOTE URL to storagebox
NTFY_ENDPOINT="" # NTFY ENDPOINT for notifications
HEALTHCHECKS_ENDPOINT="" # HEALTHCHECKS ENDPOINT for notifications

for datastore in $DATASTORES; do
if rsync -Pavr "$datastore" "$REMOTE:"; then
        curl  "$HEALTHCHECKS_ENDPOINT"
        curl "$NTFY_ENDPOINT" -X POST  -d "done sync from $HOSTNAME to $REMOTE of $datastore"
else
        curl "$NTFY_ENDPOINT" -X POST  -d "BROKEN  sync from $HOSTNAME to $REMOTE of $datastore"
fi
done
```

- then configure cron to run the script as follows

```bash
0 10 * * * /usr/local/bin/sync_with_remote.sh >> /var/log/sync_with_remote.log 2>1
```


