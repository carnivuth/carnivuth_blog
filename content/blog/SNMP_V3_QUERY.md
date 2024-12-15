---
title: how to query a snmp v3 capable device
date: 2024-11-16
index: 5
---

```bash
snmpwalk -v3 -u [user] -l authNoPriv -a md5 -A [community] [node] [oid]
```

[PREVIOUS](pages/bash_automation/AWK.md) [NEXT](pages/bash_automation/BTRFS.md)
