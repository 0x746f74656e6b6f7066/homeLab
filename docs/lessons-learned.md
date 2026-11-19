# Lessons learned

Mistakes I made, how I noticed them and what I do now.

## 1. "I set the network" is not the same as "the network is set"

**What happened.** I thought both VMs were on my isolated host-only network. The first `ifconfig` and `ip a` showed otherwise:

Both VMs were on VMware's NAT network (VMnet8) and Kali also had an interface on an old host-only network (VMnet1). <!-- optionally add your original addresses -->

The vulnerable target had a route to the internet and to my Windows host.

**Why.** The VMs were imported with their own network settings, and the Kali image had two adapters. I had only assumed the settings.

**Fix.** Metasploitable moved to VMnet3. On Kali, the NAT adapter was disconnected and the other adapter moved to VMnet3.

**Rule now.** After touching any network setting I read the real addresses inside each VM, then run the isolation test in `network.md`. The address itself tells you the network: a VMware NAT address means internet access, 10.10.10.x is my lab.

## 2. Old leftovers cause confusing networks

**What happened.** An old VMnet2 from an earlier attempt existed with the host connection enabled and a 192.168.2.0 subnet. 192.168.x.0 ranges are common on home routers and can clash with them.

**Fix.** A new VMnet3 with the host adapter disabled and a 10.10.10.0/24 subnet. Old networks get removed instead of reused.

**Rule now.** Lab networks use 10.x ranges, never the same range as my home network, and nothing is reused from earlier experiments.

## 3. Write it down while it is fresh

Earlier lab attempts failed because I could not remember what I had configured. This repository, and the address table in `network.md`, replace memory.

## 4. Scans: know why each flag is there

- `nmap -sn` finds live hosts without scanning ports.
- `-p-` is needed to find services on unusual ports. On the target, 30 TCP ports were open and several of them (the random high ports) are outside the default top 1000 and would have been missed.
- Rate limiting (`--min-rate`) is acceptable in a lab and very noisy on a real network.
