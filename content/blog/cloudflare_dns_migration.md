---
date: 2024-11-28
title: Escape from self hosting in subpaths
description: My experience migrating my personal services to a cloudflare domain
tags:
    - migrations
    - cloudflare
    - ansible
    - terraform
    - homelab
draft: false
---

In my daily work i use some self hosted services in my personal proxmox istance managed trough [ansible](https://docs.ansible.com/ansible/latest/index.html) and [terraform](https://www.terraform.io/), some of them are exposed to the internet via reverse proxy with nginx.
To make things easy for me i have decided to host applications in **subpaths** so that when i want to add a new one i don't need to get other hostnames and the work reduces to add some configuration lines to nginx which ansible can manage with no problem.

{{< mermaid >}}
flowchart LR
A[client]
B[reverse proxy]
subgraph docker_host
C[(serivice 1)]
D[(serivice 2)]
end
A --ask for a specific subpath--> B
B -- routes traffic to the specific docker container -->docker_host
{{< /mermaid >}}
> architecture before migration

Everything worked fine till the day i decided to use [ntfy](https://ntfy.sh/) for push notifications (*beautiful service i must say*), i was able to get it running in a subpath but some features where not working (*attachment download and the web interface*) but i didn't care as long as the main service was working. As time passed by i have tried more and more services but a lot of them presented similar issues when hosted in subpaths.

There where also other problems with subpaths, the ansible playbook that manages the reverse proxy configuration was based aroud `lineinfile` and `blockinfile` modules with some context dependent logic (*if line x is present write line y*). This approach lead to error prone reasoning and non managed edge cases where the playbook can push the wrong line in the configuration and stop the reverse proxy service (*it's already happened*)

So after some time i have decided to address the elephant in the room: **no more hosting in subpaths** let's manage a public domain.

## DESIGNING THE MIGRATION

So what to do now? First thing first i have to choose a provider to host a domain that i can play with, so after some research i selected [cloudflare](https://www.cloudflare.com) as my savior from subpaths, the reason? It was the cheaper and i can manage my ddns entries with some server script that connects to the cloudflare API.

The final architecture that i want to achieve looks like this:

{{< mermaid >}}
flowchart LR
A[client]
B[(CLOUDFLARE DNS SERVERS)]
C[reverse proxy]
subgraph docker_host
D[(serivice 1)]
E[(serivice 2)]
end
A --resolve *.carnivuth.org--> B
A -- ask for a specific host --> C --routes traffic--> docker_host
{{< /mermaid >}}

Where the routing is not done using the subpath but using domain names, so i can split the reverse proxy configuration in multiple files and avoid using `ansible.builtin.lineinfile` improperly by editing lines based on context.

So after creating the dns records on the [cloudflare dashboard](https://dash.cloudflare.com) i have setup a ddclient to update the ddns records when a change in my public IP occurs:

```bash
# installed ddclient on the machine
apt install ddclient
```

> inside `/etc/ddclient.conf`
```text
protocol=cloudflare \
use=web, web=ipify-ipv4 \
password='TOKEN' \
zone=carnivuth.org \
carnivuth.org
```

The next thing to change was the reverse proxy configuration and the ansible playbook that deploys and updates it [reverse proxy configuration](https://github.com/carnivuth/labcraft/commit/d55f66c80fcf24317873a36613daff72b8add1f8) to manage multiple files configuration structure.
Also the https certificate needs to be migrated to a wildcard certificate, this way i can add multiple domain names without requesting for a new let's encrypt certificate

```bash
# install depencency packets
apt install python3-certbot-dns-cloudflare

# create configuration file and secure permissions
sudo tee /etc/letsencrypt/dnscloudflare.ini > /dev/null <<EOT
# Cloudflare API token used by Certbot
dns_cloudflare_api_token = [TOKEN]
EOT
chmod 0600 /etc/letsencrypt/dnscloudflare.ini

# ask for certificate and tested with a dryrun
sudo certbot certonly -d *.carnivuth.org     --dns-cloudflare --dns-cloudflare-credentials /etc/letsencrypt/dnscloudflare.ini     --post-hook "service nginx reload"     --non-interactive --agree-tos     --email matti200042@gmail.com
certbot renew --dry-run
```

## OUTCOMES

The new infrastructure needs more simple ansible logic when exposing new applications, it also avoids weird application behaviors when hosting in subpaths like error in generated links or ajax requests, overall it was a big success!!
