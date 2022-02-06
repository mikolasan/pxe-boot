---
description: Introduction to these pages
cover: .gitbook/assets/my-cover-2.jpg
coverY: 0
---

# Overview

Hello. I'm Nikolay Neupokoev, and this is my comprehensive tutorial about PXE Boot on BIOS and UEFI clients.

In this short tutorial I will show how to boot Live-CD type systems over the network. We are going to use Raspberry PI board as a server. For operating system I chose Raspberry Pi OS (early known as Raspbian) which is technically Debian 10 (Buster).

We will cover how to install PXE Server and boot on Legacy systems and then we upgrade our installation to also support modern UEFI firmware.

First we are going to use dnsmasq as DHCP proxy and TFTP server. But if you want to use Secure Boot (PXE for UEFI firmware), then you have to have DHCP server instead of proxy and TFTP server (again) on the same machine (the same IP address). I do exclude that other options may exist and also work, but this tutorial only covers this ground base.
