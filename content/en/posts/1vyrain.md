---
date: 2025-06-07T21:28:25+02:00
# description: ""
# image: ""
lastmod: 2025-06-07
showTableOfContents: true
tags: ["Coreboot", "ThinkPad"]
title: "Flashing a Skulls coreboot image to a ThinkPad X230 with 1vyrain"
type: "post"
---

These are my personal notes on flashing the [Skulls](https://github.com/merge/skulls) Coreboot image to a ThinkPad X230 using [1vyrain](https://github.com/n4ru/1vyrain).
This is not a tutorial, but rather a record of the steps and observations from my process.

For up-to-date, in-depth help, refer to [1vyrain](https://github.com/n4ru/1vyrain) and [Skulls](https://github.com/merge/skulls) documentation.
Use these notes at your own risk.

## Introduction

- What is Coreboot? Benefits? Downsides?
- What is Skulls? Comparison to other distributions.
- What is 1vyrain? How does it work?

## Installation

### Ensure BIOS version is compatible 

The stock BIOS version **must be** below or equal to 2.60 for the ThinkPad X230.
My BIOS version was 2.50 so I did not have to run [IVprep](https://github.com/n4ru/IVprep).

### Backup the stock BIOS

I made a backup of the stock BIOS using [flashrom](https://github.com/flashrom/flashrom).

```bash
sudo flashrom -p internal -r bios_backup.rom --ifd -i bios
```

### Host the Skulls ROM on an HTTP server

`1vyrain` supports custom BIOS images from an HTTP URL.
I downloaded the image and hosted it on a local HTTP server in another machine.

#### Download the Skulls ROM

I downloaded the compressed source archive [latest release](https://github.com/merge/skulls/releases/latest) from the Skulls project repository.
I used release [`1.1.2`](https://github.com/merge/skulls/tree/1.1.2) and checked its integrity running:

```bash
sha512sum --check skulls-1.1.2.tar.xz.sha512
```

Then, I had to choose between the free and non-free images for the X230.
I chose the free image because I wanted to try out [SeaVGABIOS](https://www.seabios.org/SeaVGABIOS).

I also checked the integrity of the image.

```bash
sha256sum --check x230_coreboot_seabios_free_5243dd96de_top.rom.sha256
```

#### Run the local HTTP server

I moved my chosen image to a directory in the home directory.

```bash
mv x230_coreboot_seabios_free_5243dd96de_top.rom $HOME/rom/x230.rom
```

I then ran a simple Python HTTP server on that directory.

```bash
python -m http.server 8000 --directory $HOME/rom
```

### Burn the `1vyrain` image

I downloaded the `1vyrain` ISO from the `1vyrain` project repository.
I then burned the `1vyrain` image onto a flash drive using `dd`.

```bash
dd if=1vyrain.iso of=USB_DISK bs=8M status=progress
```

### Flash the stock BIOS

I connected the X230 via Ethernet to access my local network.

I connected the flash drive via the onboard USB ports.
Then, I booted in UEFI mode with Secure Boot off from the flash drive.

The `1vyrain` live USB started correctly. 
I pressed enter to start.
When it prompted me, I chose to flash a custom BIOS from URL.
Then, I entered the URL of my self-hosted ROM.

On reboot, the Skulls boot splash appeared and SeaBIOS loaded successfully.
It booted from my SSD right into GRUB.

## Notes

### Full-disk encryption

I rebooted a few times to make sure everything was working.
On the second to third restart, GRUB appeared to hang.
However, [this blog post](https://momi.ca/posts/2021-11-07-luksgrub.html) from Anjandev Momi helped me figure it out.
The computer was not frozen. I typed the encryption password, pressed enter, and Linux Mint with full-disk encryption booted fine.
I haven't encountered this issue again.

### Hardware whitelist

Skulls has no hardware whitelist.
I'm looking forward to upgrading the internal Wi-Fi module next.

I purchased the X230 in the second-hand market, so I was surprised to discover that it had a Bluetooth module installed when I booted into Linux Mint.
It wasn't recognized before, and I hadn't opened the palm rest, so I assumed I didn't have it.

### GRUB resolution

The GRUB boot menu was too small, so I adjusted the resolution used in the graphical terminal in `/etc/default/grub` by adding the following lines:

```
GRUB_GFXMODE=1024x768x32
GRUB_GFXPAYLOAD_LINUX=keep
```

## References

- https://github.com/merge/skulls/tree/master/x230
- https://github.com/n4ru/1vyrain
- https://docs.python.org/3/library/http.server.html
- https://momi.ca/posts/2021-11-07-luksgrub.html
