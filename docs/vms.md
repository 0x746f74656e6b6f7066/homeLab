# Virtual machines

| VM | OS | Role | Network | Address | Credentials |
|---|---|---|---|---|---|
| Kali | Kali Linux (prebuilt VMware image) | Attacker | VMnet3 (adapter 2) | 10.10.10.128 | default image credentials, changed for real use |
| Metasploitable 2 | Ubuntu 8.04 based, intentionally vulnerable | Target | VMnet3 | 10.10.10.129 | published defaults, lab only |

Disk images live outside the repository, in a folder per VM on a separate drive.

## Kali

- Prebuilt VMware image from the Kali website, opened with "I copied it".
- Two network adapters were present in the image. Adapter 1 (NAT) is set to not connected and not connect-at-power-on. Adapter 2 is on VMnet3.
- Resources: <fill in RAM / vCPU>

## Metasploitable 2

- Distributed as a zip with a ready VMware machine.
- Must never be given a NAT or bridged adapter. It is full of known vulnerabilities by design.
- Resources: <fill in RAM / vCPU>

## Planned

| VM | Role | Phase |
|---|---|---|
| pfSense | Router and firewall between segments | 2 |
| Windows Server (evaluation) | Active Directory domain controller | 3 |
| Windows client | Domain member | 3 |
| Wazuh | SIEM | 4 |
