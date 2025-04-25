---
title: O Lab 2 DNS/DHCP
date: 2025-04-24 22:38:00 America/Los_Angeles

# Templates, Articles, Computer-Networking, or Programming
categories: [Computer-Networking,DHCP]

# any concept or entity relevant in the document can be mentioned here
tags: [DHCP]
author: kevin
description:
---

## Network Topology

![img-des](/assets/image/O%20Lab%202/topology.PNG)

## Problem/Task Description

You want to configure dynamic IP addressing for the hosts in your organization using two separate DHCP address pools. Hosts A and B are located on the subnet connected to Router Alpha, which is the DHCP server, while Host C is located on a different subnet connected to Router Beta. The first 10 addresses on each subnet should be reserved for static assignments; the remaining addresses should be available for dynamic allocation.

Your supervisor has asked you to complete the following tasks:

* Configure a pool named alpha_pool on Router Alpha to assign IP addresses to Host A
and Host B. This pool should serve the network 192.168.42.240/28 connected to the
FastEthernet 0/0 interface of Router Alpha.
* Configure another pool named beta_pool on Router Alpha to assign IP addresses to
Host C. This pool should serve the network 172.20.54.192/26 connected to the
FastEthernet 0/0 interface of Router Beta.
* Ensure that hosts served by each pool use their respective router as the default
gateway.
* Ensure that all hosts use the DNS server at 192.168.42.242.
* Ensure that all hosts use the domain name corp.local.
* Ensure that all hosts use the NetBIOS server at 192.168.42.243.

All router interfaces are operational, and the proper IP addresses have been configured. All passwords are set to cisco.

## Preliminary Knowledge

DHCP servers handle locally-sourced requests and relayed requests differently. For local requests, the `giaddr` field will not be set, and the server looks to its own ip address on the interface where the request was received on to look for a matching pool. This behavior is the same whether the server is hosted on a router or dedicated hardware. If the server does not find a matching pool, it will not respond to the request.

For relayed requests, the server relies on the `giaddr` field to do the pool-matching, as opposed to its own ip address. In this lab, DHCP request from Host C will be of relayed type because the DHCP router is not on its own subnet.

## Initial Inspection Of The Situation

From the task statement, I know the `192.168.42.240/28` network is the Host A/B network with Router Alpha's interface ip addr being `192.168.42.241` probably. On the other hand, Host C is part of `172.20.54.192/26` with Router Beta's ip addr being `172.20.54.193` probably.

The first thing is to do `show run` on Router Alpha and Beta to confirm this:

![des](/assets/image/O%20Lab%202/ol02-01.png)
_Router Alpha_

![des](/assets/image/O%20Lab%202/ol02-02.png)
_Router Beta_

Furthermore, in the `show run` output I also see that Router Alpha and Beta achieve connectivity with the use of static routes, so I know once the hosts get their ip addrs, there should be two-way connectivity between the subnets.

![des](/assets/image/O%20Lab%202/ol02-03.png)
![des](/assets/image/O%20Lab%202/ol02-04.png)
_recursive static routes on both routers for connectivity_

Since Router Alpha is hosting the DHCP server in this situation, most of the configuration will be done on this router. However, I see that Host C is part of a remote subnet from Router Alpha, so I take mental note to remember to configure Router Beta to forward DHCP requests to Alpha instead.

> since `ip dns server` cmd is not supported in packet tracer, this lab does not involve dns server configuration. A dedicated dns server sits on the side as a compromise.
{: .prompt-info }

## Carrying Out The Plan

For using cisco router as DHCP server, there are 2 related but structurally independent command series. The first cmd `ip dhcp pool [pool name]` brings the router into dhcp-config mode, kind of like with named ACL or routing protocol configs:

![des](/assets/image/O%20Lab%202/ol02-05.png)

then in the dhcp-config mode there are options to communicate additional information to DHCP clients, such as default gateway ip, dns-server ip, etc. A typical configuration could look something like this:

```cisco_ios
ip dhcp pool [pool name]
network [network addr] [network mask]
default-router [router interface ip]
dns-server [server ip]
domain [domain name]
```

> the `netbios-name-server` cmd was omitted in this lab because it is not supported in packet tracer
{: .prompt-info }

the second cmd is a standalone that excludes an address range from being used in the pool:

```cisco_ios
ip dhcp excluded-address [start ip addr] [end ip addr inclusive]
```

so combining the above commands, I configured Router Alpha as follows:

![des](/assets/image/O%20Lab%202/ol02-06.png)

Then to configure Router Beta to forward the DHCP requests it receives to Alpha:

![des](/assets/image/O%20Lab%202/ol02-07.png)

I chose the point-to-point interface on Router Alpha because it was reachable on the directly connected serial link from Router Beta, but since there are static routes between Alpha and Beta, using Alpha's other `192.168.42.241` should work too.

Out of curiosity, I edited the helper address like so:

![des](/assets/image/O%20Lab%202/ol02-08.png)

## Verification



## Final Thoughts


