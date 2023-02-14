---
title: "How to use NMAP for information gathering"
date: 2023-02-11
draft: false
description: "nmap tutorial"
slug: "nmap"
tags: ["kali-linux", "cyber-security", "nmap", "pentesting-tool"]
showEdit: false
showAuthor: true
---
{{< lead >}}
When it comes to hacking, knowledge is power. That is why gathering information about the target system is the very first step in pentesting. The most popular tool to do it is NMAP. This article will teach you how to use it in Kali LInux.
{{< /lead >}}

### What is NMAP?
`nmap`, short for "Network Mapper" is a go to tool for reconessance. It scans the ports that are used in target system and checks the services running on those ports. You can use it to scan the machines in the same network or even web application which are hosted far from you. The only thing you need to know is the `IP address` of the targets. Targets with poorly implemented security will reveal 

### Ports and Services
Every servers or computers have total of 65535 ports. The first 1024 ports are reserved for system. If you are familiar with web application development, notice the server runs on port `:8080` and client on `:3000`, not between 1-1024. 

Most used ports numbers and services:

| service | port no |
| :---:  | :---:  |
| HTTP | 80 |
| HTTPS | 443 |
| NETBIOS | 139 |
| SMB | 445 |
| FTP | 20, 21 |

### Using NMAP
Using `nmap` is very straight forward. The following command will scan the IP address provided. In this tutorial, I'll use 'www.owasp.org' for the example. Before, I used [whatsmyid](https://www.whatismyip.com/) to find out the IP address.

```go
$ nmap 104.22.27.77
```

{{< figure
    src="nmap1.png"
    alt="nmap"
    default=true
    >}}

The result showed information such as PORT number, STATE of the port and what services were running. It also mentioned 996 tcp ports were filtered.

### How about I only wanna to scan specific port

In addition to IP address, you can add flags for more specific scanning. If you only want to scan port 20, you can do it as:

```go
// -p must be followed by the number
$ nmap -p 80 104.22.27.77
```

To scan ports range from 80-100:
```go
$ nmap -p 80-100 104.22.27.77
```

If the default output does not include enough information, you can add `-v` for verbosity. Verbosity has 2 levels. Single v for level one and, double for level two. In the example photo, I used `-vv` flag for level two verbosity. You can see it has more information such as REASON compare to the first output.


{{< figure
    src="nmap2.png"
    alt="nmap"
    default=true
    >}}

There are many other -flags for more features. There are very simple and easy to learn. You can read the instruction by `nmap -h` or `man nmap`.