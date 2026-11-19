# homelab

Personal offensive-security lab

Everything here is done on machines I own, on a network that has no route to the internet or to my home LAN.

## Current status

| Phase | Description | Status |
|---|---|---|
| 1 | Isolated host-only network, Kali attacker, Metasploitable 2 target | Done |
| 1b | Host discovery and full TCP port scan of the target | Done |
| 1c | Service/version detection (`-sC -sV`) and first attack paths | Next |
| 2 | pfSense between segments (attack / targets / management) | Planned |
| 3 | Windows Server Active Directory lab (Kerberos, lateral movement) | Planned |
| 4 | Wazuh SIEM, to review my own attacks from the blue-team side | Planned |
| 5 | ICS/OT segment (OpenPLC, Modbus) following the Purdue model | Planned |

## Lab at a glance

```
 Windows 11 host (VMware Workstation Pro)
 |
 +-- VMnet3  host-only, 10.10.10.0/24, no host adapter, DHCP by VMware
       |
       +-- Kali Linux          10.10.10.128   attacker
       +-- Metasploitable 2    10.10.10.129   intentionally vulnerable target
```

The NAT adapter of the Kali VM is disabled, and the target has no other network. Details in [`docs/network.md`](docs/network.md) and [`docs/vms.md`](docs/vms.md).

## Repository layout

```
homelab/
 |-- README.md
 |-- .gitignore
 |-- docs/
 |    |-- network.md           segments, addressing, isolation test
 |    |-- vms.md               every VM: OS, role, resources, network
 |    `-- lessons-learned.md   what went wrong and how I fixed it
 `-- writeups/
      |-- 00-template.md       template for every exercise
      `-- 01-recon-metasploitable2.md
```

## Rules I follow

- Only systems I own, on an isolated network.
- No secrets, keys or real network details in this repository (see `.gitignore`).
- VM disk images never go into Git; only text.
- Each exercise gets a write-up that includes what did **not** work.

## Tools

VMware Workstation Pro, Kali Linux, Metasploitable 2, nmap.
