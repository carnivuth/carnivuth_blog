---
title: terraform loops
date: 2024-11-17
index: 16
draft: true
---

Loops can be defined  specifying the variable `count` inside a specific resource, then the index of the loop  can be accessed via the `count.index` variable

```terraform
resource "proxmox_lxc" "ct-test" {
	// sets the  count variable to limit iterations
	count = 14
	// use the index to differentiate the hostname of the container
	hostname        = "ct-test-${count.index}"

	// ....

	rootfs {
		storage = var.main_pool
		size    = "8G"
	}

	network {
		name   = "eth0"
		bridge = "vmbr0"
		// use the index to differentiate the ip address
		ip     = "192.168.1.20${count.index}/24"
		gw     = var.guest_gw
	}
}
```

this will produce the following output on the proxmox host

![](/images/Pasted%20image%2020241117151330.png)


