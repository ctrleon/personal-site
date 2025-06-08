---
date: 2025-06-07T21:28:25+02:00
# description: ""
# image: ""
lastmod: 2025-06-07
showTableOfContents: false
# tags: ["",]
title: "Flashing a Skulls coreboot image to a ThinkPad X230 with 1vyrain"
type: "post"
---

This is not a tutorial, but rather a documentation of my personal process.
Always verify information with official sources.
Any use of my suggestions is at your own risk.

## Installation

1. Check Bios version is below or equal to 2.60. Mine was 2.50. If not, downgrade it using `1vyprep`.
2. Make a backup of the stock Bios.
    ```bash
    sudo flashrom -p internal -r bios_backup.rom --ifd -i bios
    ```
3. Get a URL for the Skulls ROM.
    - Download the .tar
    - Get the free or non-free image. I chose the free image.
    - Check its integrity
    - Host it online. I hosted it on my home network on an HTTP server in another computer.
    ```bash
    python -m http.server 8000 --directory ROM_DIR/
    ```
4. Burn the `1vyrain` image onto a flash drive using `dd`.
    ```bash
    dd if=1vyrain.iso of=USB_DISK bs=8M status=progress
    ```
5. Boot in UEFI mode from the flash drive, with Secure Boot off via onboard USB ports.
6. Select the choice to flash a custom BIOS from URL. Then enter the URL of the Skulls ROM, in my case: `192.168.0.119:800/x230.rom`.
7. On reboot, the Skulls boot splash appeared and SeaBIOS loaded successfully.
8. After selecting my SSD with Linux Mint 22 (with full-disk encryption), GRUB appeared to hang. A blog post revealed this is a known issue: the password prompt is invisible. Typing the password blindly allows the system to boot.
9. The post also explains that a permanent fix requires compiling a custom kernel with framebuffer support for Coreboot, since Linux Mint 22’s default kernel lacks the necessary modules.

## References

- https://github.com/merge/skulls/tree/master/x230
- https://github.com/n4ru/1vyrain
- https://docs.python.org/3/library/http.server.html
- https://momi.ca/posts/2021-11-07-luksgrub.html
