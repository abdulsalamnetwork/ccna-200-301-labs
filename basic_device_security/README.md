# Basic Device Security (CCNA)

## Overview
This lab is part of my CCNA 200-301 study track, built and simulated in **Cisco Packet Tracer**. It focuses on fundamental **device hardening** practices on a Cisco router — setting hostnames, securing privileged EXEC access, encrypting passwords, and saving the configuration to NVRAM.

**File:** `Day 04 Lab - Basic Device Security.pkt`

## Topology

| Device | Model      | Role            |
|--------|-----------|-----------------|
| R1     | Cisco 2911 | Router          |
| SW1    | Cisco 2960-24TT | Switch    |
| PC1    | PC-PT     | End device      |
| PC2    | PC-PT     | End device      |
| PC3    | PC-PT     | End device      |

**Layout:** R1 connects to SW1, which fans out to PC1, PC2, and PC3.
![Layout](screenshots/.png)
```
        PC1
         |
R1 ---- SW1---- PC2
         |
        PC3
```

## Objectives
- Set a hostname on the router
- Configure and encrypt privileged EXEC passwords (`enable secret` and `enable password`)
- Apply `service password-encryption` to obscure weak (type 7) passwords in the config
- Save the running configuration to startup configuration (NVRAM)
- Verify configuration using `show running-config` / `show startup-config`

## Configuration Steps Performed on R1

```
R1(config)# hostname R1
R1(config)# enable secret <secret-password>
R1(config)# enable password <backup-password>
R1(config)# service password-encryption
R1(config)# ip cef
R1(config)# no ipv6 cef
```

Then the configuration was saved:

```
R1# write memory
R1# write me
```

## Verification

Confirmed the saved configuration with:

```
R1# show startup-config
```

Key output confirmed:
- `hostname R1`
- `enable secret 5 <encrypted-hash>` — securely hashed (MD5-based, type 5)
- `enable password 7 <encrypted-hash>` — obscured via type 7 encryption (`service password-encryption`)
- `ip cef` enabled, `ipv6 cef` disabled

## Key Learning Points
- **`enable secret` vs `enable password`** — `enable secret` is always hashed (stronger, MD5) and takes priority over `enable password` if both are set. `enable password` is stored in plaintext by default unless `service password-encryption` is applied (and even then it's only weak type 7 obfuscation, not real encryption).
- **`service password-encryption`** — encrypts (weakly) all plaintext passwords currently in the config and any added afterward; it's a deterrent against casual shoulder-surfing, not real security.
- **`write memory` (or `copy running-config startup-config`)** — required to persist configuration across reboots; without it, changes are lost on power cycle.
- **`ip cef`** — Cisco Express Forwarding, enabled by default on most modern IOS routers for faster Layer 3 switching.


## Tools Used
- Cisco Packet Tracer
- FREE CCNA 200-301 course material Jeremy's IT lab

---
*Part of my personal CCNA 200-301 lab series — Day 04.*
