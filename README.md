# MT7601U Access Point Capable Driver (mac80211)

This repository contains a modified version of the mainline Linux `mt7601u` wireless driver, extending it to add support Access Point (AP) mode. 

Unlike the older, unmaintained legacy vendor driver (`mt7601Uap`), this implementation is **fully compatible with the modern Linux network stack (`mac80211`)**. It seamlessly integrates with standard Linux networking tools like `hostapd`, `wpa_supplicant`, and `NetworkManager`.

>Note: This project is experimental and is not mature or stable enough for daily use. You may encounter bugs, crashes, lockups, or other unexpected behavior. Use it at your own risk.

**Why This Repository?**
For testing purposes, I copied the modified drivers/net/wireless/mediatek/mt7601u source directory from the Linux kernel source tree into this repository.
There is intentionally no custom Makefile. The kernel's existing kbuild infrastructure is used to compile the driver as an out-of-tree module.

## Building and Installation

You can compile this module completely out-of-tree without rebuilding your entire kernel. Go to the root of this cloned directory and run the appropriate build command for your compiler:

**For GCC:**
```bash
make -C /lib/modules/$(uname -r)/build M=$(pwd) modules

```

**For LLVM/Clang:**

```bash
make -C /lib/modules/$(uname -r)/build M=$(pwd) LLVM=1 modules

```

**To load the driver:**
Unload the existing mainline module and insert the newly compiled one:

```bash
sudo rmmod mt7601u
sudo insmod mt7601u.ko

```

## Testing and Compatibility

Currently, this is only tested on my Arch machine with `7.1.7-arch1-1` kernel, but since the modified source is from a 6.x kernel it should build fine for 6.x and 7.x versions.

The modified driver source was taken from a Linux 6.x kernel source tree, so compatibility with older kernel versions is not guaranteed.

If you test this driver on a different kernel version, please report whether it builds and works correctly.

## Issues and Contributions

If you test this and encounter any crashes, bugs or lockups, **please open an issue**. Log outputs from `dmesg`, `hostapd`, and `iw` are highly appreciated.

## License

This software is a derivative work of the Linux Kernel and is released under the **GNU General Public License v2.0 (GPLv2)**, identical to the mainline kernel source.
