![OpenWrt logo](include/logo.png)

# OpenWrt for OrangePi RV2

This is an experimental fork that adds support for the OrangePi RV2 (RISC-V) board to OpenWrt 25.12. The main OpenWrt tree doesn't support this board yet, so this is my attempt to bring it up to date.

---

## ⚠️ Big Fat Warning

**This is testing software. Flash at your own risk.**

I'm not responsible if you brick your board, corrupt your NVMe drive, or your device suddenly becomes a paperweight. If something breaks, you get to keep both pieces. Make sure you have a recovery plan before flashing.

That said, it works for me. Your mileage may vary.

---

## What This Is

I ported OrangePi RV2 support from OpenWrt 24.10 over to OpenWrt 25.12. The main OpenWrt project uses kernel 6.12 now, but the vendor drivers for this board are deeply tied to kernel 6.6. Rather than fight with hundreds of rejected patches, I kept it on 6.6 where everything actually works.

### What Works

- **WiFi** - Broadcom bcmdhd driver
- **Ethernet** - Both the built-in emac and the RTL8125 2.5GbE
- **NVMe boot** - Booting from NVMe SSD works
- **Display** - DRM/KMS graphics
- **GPIO** - I2C, SPI, UART, device tree overlays
- **Docker** - containerd and docker-compose included

---

## Building It Yourself

If you want to compile this from source, here's the drill.

### What You Need

A Linux machine (or WSL with case-sensitive filesystem). These packages:

```
binutils bzip2 diff find flex gawk gcc-9+ getopt grep git install libc-dev
libz-dev make4.1+ perl python3 rsync subversion unzip which
```

### The Build Process

```bash
# 1. Grab all the package definitions
./scripts/feeds update -a

# 2. Install them as symlinks
./scripts/feeds install -a

# 3. (Optional) Tweak the config
make menuconfig
# Target System → Ky
# Subtarget → riscv64
# Target Profile → x1 boards (64 bit)

# 4. Download all the source code
make download

# 5. Build it (grab a coffee, this takes a while)
make -j$(nproc)
```

When it's done, you'll find the images in `bin/targets/ky/riscv64/`.

---

## Flashing

Grab either `openwrt-ky-riscv64-x1_orangepi-rv2-ext4-sysupgrade.img.gz` or the squashfs version. 

Decompress and write to your NVMe drive or SDCARD:

```bash
gunzip openwrt-ky-riscv64-x1_orangepi-rv2-ext4-sysupgrade.img.gz
dd if=openwrt-ky-riscv64-x1_orangepi-rv2-ext4-sysupgrade.img of=/dev/your-nvme bs=4M status=progress
```

---

## Issues?

This is a side project. Things might break. If you find bugs or have fixes, feel free to open an issue or submit a pull request. No promises on response time, but I'll do my best.

**Join the Telegram community:** https://t.me/OrangePiRV2

---

## Original OpenWrt Info

OpenWrt Project is a Linux operating system for embedded devices. Instead of a static firmware, it gives you a fully writable filesystem with package management. You're not stuck with what the vendor gave you - customize it to fit your needs.

### Links

- [OpenWrt Website](https://openwrt.org)
- [Forum](https://forum.openwrt.org)
- [Documentation](https://openwrt.org/docs)

## License

GPL-2.0
