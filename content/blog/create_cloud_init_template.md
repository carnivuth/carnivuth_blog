---
title: how to create proxmox cloud init templates
date: 2024-09-16
id: CREATE_CLOUD_INIT_TEMPLATE
aliases: []
tags: []
draft: true
---

In order to use `terraform` as a [IaC](https://it.wikipedia.org/wiki/Infrastructure_as_Code) tool in a `proxmox` cluster [cloud init templates](https://pve.proxmox.com/wiki/Cloud-Init_Support) are needed to generate vms from, in order to generate a cloud init template:

- download cloud init image template (*ubuntu 24.04 in this example*)

```bash
cd /tmp/
wget https://cloud-images.ubuntu.com/noble/current/noble-server-cloudimg-amd64.img
virt-customize -a noble-server-cloudimg-amd64.img --install qemu-guest-agent
```

- create vm that will become the template and inport disk

```bash
qm create 9000 --name "ubuntu-2404-cloudinit-template" --memory 2048 --cores 2 --net0 virtio,bridge=vmbr0
qm importdisk 9000 noble-server-cloudimg-amd64.img local-lvm
```

- set scsi device emulation and other parameters

```bash
qm set 9000 --scsihw virtio-scsi-pci --scsi0 local-lvm:vm-9000-disk-0
qm set 9000 --boot c --bootdisk scsi0
qm set 9000 --ide2 local-lvm:cloudinit
qm set 9000 --serial0 socket --vga serial0
qm set 9000 --agent enabled=1
```

- convert vm to template

```bash
qm template 9000
```


