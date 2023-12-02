---
layout: post
title:  "TIL: Unlocking Internal URLs with SSH Proxy"
date:   2023-12-02
categories: ssh proxy url internal firefox socks
---

[What is a TIL?]({% post_url 2023-11-28-til-today-i-learned %})

Today I learned a neat trick: if your institution doesn't support VPN, you can still access internal URLs using an SSH proxy. Here are the steps:
<!--more-->
1. Open a terminal and run `ssh -ND 9999 user@hostname.domain`.
2. In Firefox, change the SOCKS settings to localhost, port 9999 and SOCKS v5.

Voila! No VPN? No problem. Access internal URLs easily with this SSH proxy workaround.
