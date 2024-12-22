---
title: how to query a snmp v3 capable device
date: 2024-11-16
index: 5
draft: true
---

```bash
snmpwalk -v3 -u [user] -l authNoPriv -a md5 -A [community] [node] [oid]
```


