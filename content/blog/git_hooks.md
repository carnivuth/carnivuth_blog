---
date: 2024-10-25
title: automate systems with git hooks
aliases: []
tags: []
draft: true
---

Git hooks are a special git feature that allow for script execution when some particular event is fired.

To create a git hook create a script with the name of the event that should fired the script inside the `.git/hooks` directory, script are executed **from the root dir of the git repository**, some examples are already put there by git.

```bash
ls .git/hooks/
post-commit
```

## RUN HOOK ON SPECIFIC CHANGES ON FILES

In order to run hooks only when specific files are modified git cli can be used, here and example with the `post-commit` hook

```bash
#!/bin/bash
# Redirect output to stderr.
exec 1>&2
# check if last commit contains something that has notes in the name
INCLUDED=notes
if git diff --name-only HEAD HEAD~1 | grep "$INCLUDED"; then
# do something
else
  echo "no $INCLUDED modified, nothing to do"
fi
```

## TRIGGER ACTIONS ON PULL

The best hook for triggering actions on pull is the `post-merge` hook, this can be useful when deploying git repos to production (*suitable only for small projects*).

For example in the [labcraft](https://github.com/carnivuth/labcraft) project there is a hook for running ansible after playbooks updates

```bash
#!/bin/bash
# Redirect output to stderr.
exec 1>&2
# check if last commit contains something that has notes in the name
INCLUDED=playbooks
LOG_DIR="/var/log/ansible"; if [[ ! -d "$LOG_DIR" ]];then mkdir -p "$LOG_DIR"; fi

if git diff --name-only HEAD HEAD~1 | grep "$INCLUDED"; then
        # do something
        source env/bin/activate
        ansible-playbook -i inventory/prod.proxmox.yaml -e 'vars_file=prod' playbooks/common.yml > "$LOG_DIR/common.log"
else
        echo "no $INCLUDED modified, nothing to do"
fi
```


