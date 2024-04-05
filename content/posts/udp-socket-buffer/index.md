---
title: "UDP Socket Buffer on Linux"
date: 2024-04-05
draft: true
category: "Network"
tags: ["UDP"]
mathjax: false
comments: false
---

## Introduction

This post covers monitoring UDP socket's buffer capacity and occupied size.
Based on a brilliant tutorual [UDP Socket Buffer in Linux – Baeldung](https://www.baeldung.com/linux/udp-socket-buffer).

<blockquote>
The AF_INET socket family supports the stream and datagram sockets, which represent TCP and UDP sockets, respectively. For this family, each socket comes with buffers for outgoing and incoming packets. These buffers are a region of memory that’s used to temporarily store the packet before it’s consumed or transmitted. When there are more packets received than the buffer can fit, the kernel has no choice but to drop all the other packets.

The Linux kernel tracks the statistics of the buffer and dropped packets of each of the sockets in the system and stores it in the /proc/net/udp pseudo-file.
</blockquote>

## 1. Monitoring the UDP Socket Buffer

### 1.2. Using the `ss` Command-Line Tool

The [`ss` command-line tool](https://man7.org/linux/man-pages/man8/ss.8.html) is a network utility to investigate sockets.
To obtain the statistics of all the listening UDP sockets, let’s run the ss command with the -ul options:

```shell
ss -ul
State          Recv-Q         Send-Q           Local Address:Port            Peer Address:Port        Process 
UNCONN         0              0                  0.0.0.0:mdns                  0.0.0.0:*
UNCONN         0              0                  0.0.0.0:40564                 0.0.0.0:*
...
```

From the output table, each row represents one listening UDP socket. Then, the Rec-Q and Send-Q columns tell us the number of packets currently in the receiving buffer and send buffer, respectively.
**When the number of Rec-Q and Send-Q is consistently growing, we can conclude that the process is struggling to process the queue.**

To further obtain its buffer memory statistics, we can additionally pass the -m option:

```shell
ss -ulm
State  Recv-Q Send-Q                      Local Address:Port          Peer Address:PortProcess                                                 
UNCONN 0      0                                 0.0.0.0:mdns               0.0.0.0:*   
	 skmem:(r0,rb212992,t0,tb212992,f4096,w0,o144,bl0,d0) 
UNCONN 0      0                                 0.0.0.0:40564              0.0.0.0:*   
	 skmem:(r0,rb212992,t0,tb212992,f0,w0,o0,bl0,d0)      
...
```

With the -m option, each row contains the skmem data structure that displays information about the buffer. The r0 and rb212992 mean that the receiving buffer has 0 packets in the queue and a 212992 byte of capacity. Then, the subsequent values contain the same information for the sending buffer. At the end of the tuple, the value with the d prefix indicates the number of packets that are dropped. Through this value, we can monitor the statistics of dropped packets on our socket.