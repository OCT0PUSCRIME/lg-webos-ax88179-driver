# ASIX AX88179 Kernel Module for LG webOS 5.0

Custom-compiled kernel driver (`ax88179_178a.ko`) for the ASIX AX88179 / AX88178A chipset (such as the TP-Link UE306 V1 / UE300), built for rooted LG webOS 5.0 TVs running kernel **`4.4.84-612.jcl4tv.40`** (e.g., 2020 `55UN7300PUF`).

This module enables USB 3.0 / Gigabit Ethernet adapters to work on older webOS 5.0 firmware that lacks built-in driver support for this chipset.

## Target Hardware & Environment

| Component | Details |
|---|---|
| **Tested TV Model** | LG 55UN7300PUF |
| **webOS Version** | webOS 5.5.0-19 (`jhericurl-jervisbay` / webOS 5.0 platform) |
| **Kernel Version** | `4.4.84-612.jcl4tv.40` (ARMv7l) |
| **Adapter Chipset** | ASIX AX88179 / AX88178A |
| **USB ID** | `0b95:1790` |
| **Tested Adapter** | TP-Link UE306 V1.6 |

---

## Quick Installation

### Prerequisites

1. A rooted LG TV running kernel `4.4.84-612.jcl4tv.40` (verify with `uname -r`).
2. SSH access to the TV.
3. [LG-USB-2-ETH0](https://github.com/ohmybahgosh/LG-USB-2-ETH0) installed via the Homebrew Channel or CLI.

### Setup Instructions

**1. Download the kernel module**

Download `ax88179_178a.ko` from this repository and copy it to your TV using SCP:

```bash
scp ax88179_178a.ko root@<TV-IP>:/home/root/
```

**2. Load the driver**

SSH into your TV and load the kernel module:

```bash
insmod /home/root/ax88179_178a.ko
```

Verify that the driver loaded successfully:

```bash
dmesg | tail -n 15
```

Look for messages similar to:

```text
registered new interface driver ax88179_178a
eth1: register 'ax88179_178a'
```

**3. Configure the USB Ethernet adapter**

Use the `LG-USB-2-ETH0` script to bind the driver and promote the USB adapter to `eth0`:

```bash
sh /tmp/LG-USB-2-ETH0.sh install --module /home/root/ax88179_178a.ko --now
```

---

## Build Details

The kernel module was compiled in a Debian LXC container using the following sources and toolchain:

| Component | Details |
|---|---|
| **Kernel Source** | Official LG Open Source Release (`webOS 5.0 JKHP 2.0` / `kernel-1.0.0-612.21.tar.gz`) |
| **Driver Source** | TP-Link Linux Driver V1.19.0 (`ax88179_178a.c`) |
| **Build Environment** | Debian LXC container |
| **Toolchain** | `gcc-arm-linux-gnueabihf` (ARMv7-A 32-bit) |
| **KCFLAGS** | `-march=armv7-a` |
| **HOSTCFLAGS** | `-fcommon` |
