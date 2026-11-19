# Network design

## Segments

| Name | Type | Subnet | Host adapter | DHCP | Purpose |
|---|---|---|---|---|---|
| VMnet3 | Host-only | 10.10.10.0/24 | Disabled | VMware local DHCP | Lab network: attacker and targets |

"Host adapter disabled" means the Windows host itself has no interface on this network. The vulnerable machines cannot reach the host, and the host cannot be reached from them.

Other virtual networks that exist on the host (VMnet1 host-only and VMnet8 NAT) are **not** used by any lab VM.

## Addressing

| Machine | Interface | Address | Note |
|---|---|---|---|
| Kali Linux | eth1 | 10.10.10.128 | eth0 (NAT) is disconnected |
| Metasploitable 2 | eth0 | 10.10.10.129 | only interface |
| VMware DHCP | - | 10.10.10.254 | answers the host discovery scan |

Addresses are assigned by DHCP and can change. If something stops working, check that the addresses are still these.

## How the network was built

1. VMware Workstation Pro: Edit, then Virtual Network Editor, then Change Settings (needs admin).
2. Add Network, choose a free VMnet (VMnet3 here).
3. Type: Host-only.
4. Untick "Connect a host virtual adapter to this network".
5. Tick "Use local DHCP service".
6. Subnet IP 10.10.10.0, mask 255.255.255.0.
7. For each VM: Edit virtual machine settings, Network Adapter, Custom: VMnet3.

## Isolation test

Run from the Kali VM:

```bash
ip a                              # one interface with a 10.10.10.x address
ping -c 4 10.10.10.129            # target: MUST answer
ping -c 4 8.8.8.8                 # internet: MUST fail
```

Result log:

- [ ] Target answers
- [ ] Internet unreachable

Tick both boxes only after running the commands.

## What I check before every session

- Only one adapter per VM has an IP in 10.10.10.0/24.
- No VM has a NAT or bridged adapter connected.
- The target VMs are powered off when I am not using them.
