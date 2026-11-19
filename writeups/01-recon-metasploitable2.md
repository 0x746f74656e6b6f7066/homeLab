# Reconnaissance of Metasploitable 2

- **Date:** YYYY-MM-DD  <!-- fill in -->
- **Lab phase:** 1b
- **Target:** Metasploitable 2 (10.10.10.129)
- **Attacker:** Kali Linux (10.10.10.128)

## Goal

Discover what is alive on the lab network and map every open TCP port on the target, following the order a real engagement would use: discover, map, identify.

## Setup

Isolated host-only network VMnet3 (10.10.10.0/24). See [`../docs/network.md`](../docs/network.md).

## Steps

### 1. Host discovery

```bash
nmap -sn 10.10.10.0/24
```

Three hosts answered:

| Address | What it is |
|---|---|
| 10.10.10.129 | Metasploitable 2 (target) |
| 10.10.10.254 | VMware DHCP service |
| 10.10.10.128 | The Kali VM itself (no MAC shown because it is the scanning machine) |

### 2. Full TCP port scan

```bash
sudo nmap -p- --min-rate 1000 10.10.10.129 -oN all-ports.txt
```

Nmap reported 65505 closed ports out of 65535, so **30 ports are open**:

```
21 ftp        22 ssh        23 telnet      25 smtp       53 domain
80 http       111 rpcbind   139 netbios    445 smb       512 exec
513 login     514 shell     1099 rmiregistry  1524 ingreslock
2049 nfs      2121 ftp      3306 mysql     3632 distccd  5432 postgresql
5900 vnc      6000 X11      6667 irc       6697 ircs-u   8009 ajp13
8180 unknown  8787 msgsrvr  + 4 high ports (random on each boot)
```

The four high ports change on every boot and fall outside the default top-1000 scan, so only a full `-p-` scan finds them. <!-- replace with the exact numbers from your own output -->

### 3. Service and version detection (next step)

```bash
sudo nmap -sC -sV -p <comma-separated list of the open ports from step 2> 10.10.10.129 -oA detailed
```

Planned analysis: pick the three services I would attack first and explain why, based on the version strings.

## Result

A complete list of open ports on the target. Versions and attack paths are the next write-up.

## What did not work

My first attempt at the lab had both machines on the wrong network (see [`../docs/lessons-learned.md`](../docs/lessons-learned.md)). Scanning from there would have mixed in other hosts and would have exposed the vulnerable machine.

## Defender's view

A scan like this is very visible: a single source touching tens of thousands of ports in seconds. Any IDS or firewall log with connection counting per source would flag it. `--min-rate 1000` makes it louder, and that is acceptable only inside a lab.

## Mitigations

On a real host: close services that are not needed, filter the rest with a host firewall, and segment the network so that a compromised machine cannot scan everything.
