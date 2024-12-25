---
date: 2024-09-16
title: Make industrial cronjobs
draft: false
tags:
    - cron
---

From times to times when i step into some remote server, at work or for my personal services i need to write or manage some tasks that runs continuously as cronjobs and i have collected some best practices from my experience and what colleagues had teach me

## MAKE THEM PORTABLE

From my experience cronjobs are often write in no time situations as a way to add a respond quickly to a client request and when designing big infrastructure migrations they always get in the way (no existent paths, no dependency installed or no environment variable set are the most common reasons for things to brake), in order to avoid this situations:

- use folders that are known to exists (like `/var/log` or `/var/lib`)
- create folders if they do not exists (*here a code snippet for reference*)

```bash
MY_FOLDER="/non/existant/folder"; if [[ ! -d "$MY_FOLDER" ]]; then mkdir -p "$MY_FOLDER"; fi
```

## MAKE DEBUG PRODUCTION EASY: WRITE LOGS

**Cronjob must be pedantic !!** one of the most uncomfortable situation is to debug a cronjob in production that manages a critical part of the infrastructure and the script does not write anything in standard-output nor to the filesystem itself and the only way trough it is to browse the source code that has seen little to no comments and it's a complete mess of `if/else` statements, `ssh to-unknown-machines` or the worst of all: `exec of other language interpreters` like:

```bash
# some extreme complicated logic
my_var="$(python other_script.py)"
# other logic that uses my_var
```

So log things is a must, especially when scripting commands like `ssh` or `curl` that relies on the infrastructure to be configured in a specific way for example ssh keys installed on destination or http traffic allowed by firewall wich is not always guaranteed

My personal taste is to log in stdout and then redirect output to some file under `/var/log` (for example `/var/log/$0`) or to the `logger` command even if this solution is subject to the log configuration, this way i can  see logs when debugging by running command in foreground and find information of previous runs in logfiles

## NOTIFICATION AND MONITORING

A big part of making cronjob is monitoring them and manage notifications, for example if i write a backup cronjob i want it to notify me when it breaks and when a backup is completed, but i don't want to be flooded by cronjob notifications or i will ignore them so notify all and only the important stuff is a must.

Personally i don't like mail notifications, they need some additional automation to avoid manually reorganize the mailbox and avoid missing them, at my home i use ntfy, a notification service that sends push notifications to my devices when things breaks

## RECAP

So here are some common best practices to write functional cronjobs:

- log every command that is useful like `ssh "someserver" || echo "ssh failed for some reason, maybe no ssh key installed?" >> /var/log/"$(basename $0)".log`
- notify important actions or events
- use the script name for logs and files in the file system for quick grep command when debug is needed
- use known folders or create folders that does not exists

Here i post a quick snippet for quick access

```bash
SCRIPT_NAME="$(basename "$0")"
RESULT_FILE="/var/run/$SCRIPT_NAME.result"
HEALTHCHECK="<HEALTHCHECK_URL>"

# setup result file for healthcheck
echo "0" > "$RESULT_FILE"

# core business

# curl healthchek.io
if [[ "$(cat "$RESULT_FILE")" == "0" ]]; then
    curl "$HEALTHCHECK"
fi
```

it's also usefull to avoid mail notification from the cron daemon:

```cron
*/5 * * * * //usr/local/bin/some_script.sh > /var/log/some_script.log 2>&1
```


