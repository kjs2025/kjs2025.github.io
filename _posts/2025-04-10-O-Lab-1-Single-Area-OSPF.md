---
title: Lab 1 single-area OSPF
date: 2025-04-10 11:50:00 America/Los_Angeles

# Templates, Articles, Computer-Networking, or Programming
categories: [Computer-Networking,OSPF]

# any concept or entity relevant in the document can be mentioned here
tags: [OSPF]
author: kevin
description:
---

## Network Topology

![img-des](/assets/image/O%20Lab%201/Network-topology.PNG)

## Problem/Task Description

You are a network administrator at a growing research firm that has recently expanded into two new office buildings. Each building is connected via a WAN link, and internal connectivity is provided via FastEthernet and Serial links.

Your task is to implement OSPF routing across the network. All router interfaces are up, and the correct IP addresses have been assigned to all router and end device interfaces.

Your manager has provided the following objectives:
* Configure an OSPF process using process ID 10 on RouterX, RouterY, RouterZ, and
RouterW.
* Ensure that each router advertises only the networks connected to its interfaces.
* All routers should participate in OSPF Area 0.
* Test full end-to-end connectivity by verifying that Host1 can successfully ping Host2.

You may also verify your configuration using the show ip route and show ip ospf
neighbor commands to confirm OSPF is working as expected.

## Auxiliary Comments On The Lab

To provide a little context, these labs are replicas of the labs on the boson practice CCNA exams, they are meant for exam preparation and differ greatly in style from the exploratory labs I would do myself on a given topic. Specifically, they:

1. do not offer annotated network topology diagrams, there are no network or ip or interface information on the visuals. All info is collected through show commands of various sorts by the lab taker.
2. are multi-focal in that a single lab covers could cover up to 3 CCNA topics in one lab, but 3 is a lot, usually 2, rarely 1. This lab is just warm up so it covers just single-area OSPF.
3. are either troubleshooting oriented or config-based. This will be made obvious in the language used in the task statement.
4. offer a set of assumptions to start people off with. This is supposed to save time because on the exam these labs get around 10 minutes each, so not a whole lot of time to troubleshoot problems from the bottom up. For instance, in this lab they say:
>"All router interfaces are up, and the correct IP addresses have been assigned to all router and end device interfaces."
5. try to mimic real life situation with the language used and sometimes they put irrelevant devices and links, put passwords on things (fair), etc.
6. super old school. There are serial links and like NetBIOS servers, who even use these things anymore??

## Initial Inspection Of The Situation

I started with the hosts, issuing

```shell
ipconfig
```

and checking if they can ping their gateway.

![des](/assets/image/O%20Lab%201/ol01-1.png)
_host 1_

| name  | ip addr        | gateway addr | can ping |
| :---- | :------------- | :----------- | :------- |
| host1 | 192.168.30.100 | 192.168.30.1 | yes      |
| host2 | 192.168.50.100 | 192.168.50.1 | yes      |

then I went on the routers and scanned through the `show run` logs. The annoying thing is since the topology diagram is not annotated, I had to kinda guess at the the ip/interface mapping. 

For instance, on Router-Y, I see that interface fa0/1 has the gateway ip for host 1, so I know that link goes to the host.

![des](/assets/image/O%20Lab%201/ol01-2.png)

On Router-X, I see the serial interface and the clock rate entry below tells me that Router-X has the DCE (Data Communication Equipment) side of the serial link.

![des](/assets/image/O%20Lab%201/ol01-3.png)

> for serial connections, there are the DCE and DTE sides, only DCE side sets the clock rate
{: .prompt-info }

I also know it's a point-to-point link because of the .252 subnet mask (only 2 usable hosts on a .252 == /30 network)

Anyway, so I go around and collect basic information about the network, and try to get familiar with things. That's how I would start in any situation.

## Carrying Out The Plan

Reading the task statement, I take mental note that my OSPF process-ID is 10, and that all routers should be in area 0. So I go around and type in variations of the basic OSPF configuration:

```cisco_ios
router ospf 10
network x.x.x.x wildcard-mask area 0
network y.y.y.y wildcard-mask area 0
```

For instance, on Router-X I have configured:

![des](/assets/image/O%20Lab%201/ol01-4.png)

the configuration itself is simple enough, but the setup of the network was kind of weird. The Router-X~Y link used a `/24` subnet for some reason, but the Router-Z~W link used a `/30`, which made more sense since it is a point-to-point situation. Good thing in packet tracer I can still use the `ctrl+a` shortcut to jump to the beginning of a line so combined with cmd history scrolling I can easily do things like:

```cisco_ios
no network 192.168.20.0 0.0.0.3 area 0
network 192.168.20.0 0.0.0.255 area 0
```

## Verification

Finally, as instructed, I would verify connectivity by a ping from host 1 to host 2:

![des](/assets/image/O%20Lab%201/ol01-5.png)

## Final Thoughts

It was not too bad for a warm up lab. I had recruited ChatGPT's help in the replication of these labs, feeding it excerpts from the real boson task statements, and specifying a format to communicate the network topology requirements so it could also understand what I was looking at. That was an interesting experience in itself. 

As for the lab, I suppose this is how these labs feel. Because of the assertions the task statement gave me on the correctness of assigned IP and interface setups, technically, I could have just jumped in and configured OSPF on all the routers and called it a day without the preinspection I did, scanning the network and all. But...doing that makes me somewhat uncomfortable because I'm used to interacting with situations on an intimate level, fishing out details here and there and working based off rich understanding of the context. On the other hand, there will be time pressure on the CCNA, and there was a good reason why they would routinely provide assertions like that to save my time...

I guess its a meet in the middle situation. I think preinspections are still helpful, because it's important that I fight the urge to rush everything, and give myself fair time to read things and process the situation, especially considering not all labs will be this easy. I just have to get used to doing it quickly, and that merely takes practice.
