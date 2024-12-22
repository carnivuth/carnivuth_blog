---
date: 2024-09-16
title: make cronjobs like a pro
aliases: []
tags: [CRON]
index: 1
---

# CREATE CRON JOB

here a quick reference for creating a cronjob for industrial usage,

## BEST PRACTICES

Common best practices to improve sysadmin operations:

- create script under `/usr/local/bin`
- log under `/var/log`
- creates temp files under `/tmp`
- use the script name for logs and files in the file system for quick grep command when debug is needed
- specify dependencies in a deps variable such as `DEPS="curl git jq"`
- curl [healthchecks.io](https://healthchecks.io) for monitoring (*optional*)
- script should also log every possible error like `ssh "someserver" || echo "ssh failed for some reason, maybe no ssh key installed?" >> /var/log/$SCRIPT_NAME.log`

## SNIPPET

```bash
DEPS='curl'
SCRIPT_NAME="$(basename "$0")"
LOG_FILE="/var/log/$SCRIPT_NAME.log"
RESULT_FILE="/var/run/$SCRIPT_NAME.result"
HEALTHCHECK="<HEALTHCHECK_URL>"

# setup result file for healthcheck
echo "0" > "$RESULT_FILE"

# core business

# curl healthchek.io
if [[ "$(cat "$RESULT_FILE")" == "0" ]]; then
    curl "$HEALTHCHECK" >> "$LOG_FILE" 2>&1
fi
```

it's also usefull to avoid mail notification from the cron daemon:

```cron
*/5 * * * * //usr/local/bin/some_script.sh > /dev/null 2>&1
```


