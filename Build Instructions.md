# Build instructions to build OpenWrt for Ubiquiti EdgeRouter X (ER-X) and Ubiquiti EdgeRouter X-SFP (ER-X-SFP)

## Prerequisite

Based on ubuntu-20.04.2-live-server-amd64.iso.

```
sudo apt update
sudo apt install build-essential clang flex bison g++ gawk \
gcc-multilib g++-multilib gettext git libncurses-dev libssl-dev python2 rsync swig unzip zlib1g-dev file wget

```

## Download

```
git clone https://github.com/rein2610/openwrtedgerouter.git -b edgerouterupgrade

```

## Update the feeds
```
cd openwrtedgerouter
./scripts/feeds update -a
./scripts/feeds install -a
make defconfig

```

## Configure the firmware image

```
make menuconfig

```


Target System (MediaTek Ralink MIPS)  
Subtarget (MT7621 based boards)  
Target Profile (Ubiquiti EdgeRouter X) or (Ubiquiti EdgeRouter X-SFP)

Under Global Build settings:  
- Under Kernel build options:
    - [ ] Cryptographically signed package lists
    - [ ] Enable signature checking in opkg
    - [ ] Compile the kernel with debug filesystem enabled  
    - [ ] Compile the kernel with symbol table information (NEW)  
    - [ ] Compile the kernel with debug information  
    - [ ] Enable process core dump support (NEW)  
- [ ] Enable IPv6 support in packages 
- [*] Strip unnecessary exports from the kernel image (NEW)
- [*] Strip unnecessary functions from libraries (NEW)

Under Base system:
 - < > opkg................................................ opkg package manager  ----

Under LUCI:
- Under 1. Collections:
  - <*> luci................... LuCI interface with Uhttpd as Webserver (default)
- Under 2. Modules:
    - [*] Minify Lua sources


Under Network:  
 - Under SSH:  
   - <*> openssh-sftp-server.................................. OpenSSH SFTP server



Alternative: use the provided `.config`

```
git restore .config

```

# Build
```
make -j$(nproc)

```

# Output Ubiquiti EdgeRouter X (ER-X)
```
 ll bin/targets/ramips/mt7621/
total 9196
drwxr-xr-x 3 rein rein    4096 Oct  9 18:05 ./
drwxr-xr-x 3 rein rein    4096 Oct  9 18:01 ../
-rw-r--r-- 1 rein rein    1499 Oct  9 18:01 config.seed
-rw-r--r-- 1 rein rein    2371 Oct  9 18:05 openwrt-edgerouter-upgrade-ramips-mt7621-device-ubnt-edgerouter-x.manifest
-rw-r--r-- 1 rein rein 3112960 Oct  9 18:05 openwrt-edgerouter-upgrade-ramips-mt7621-ubnt_edgerouter-x-initramfs-factory.tar
-rw-r--r-- 1 rein rein 3099229 Oct  9 18:05 openwrt-edgerouter-upgrade-ramips-mt7621-ubnt_edgerouter-x-initramfs-kernel.bin
-rw-r--r-- 1 rein rein 3174591 Oct  9 18:05 openwrt-edgerouter-upgrade-ramips-mt7621-ubnt_edgerouter-x-squashfs-sysupgrade.tar
drwxr-xr-x 2 rein rein    4096 Oct  9 18:05 packages/
-rw-r--r-- 1 rein rein     661 Oct  9 18:05 sha256sums

cat sha256sums
304808e5d44dd428a87e530bd6c5a0ced9094d9bc3bb707eb611636e03118663 *config.seed
c7165019de07b7cd3fbaca1341678fabd9f279fd00b44489c5dafcb71cf3cad2 *openwrt-edgerouter-upgrade-ramips-mt7621-device-ubnt-edgerouter-x.manifest
e98d75272e7526cabf2fc8a79ba4b40d9599bc2c8e5471d3ceb1f7fc819b70e1 *openwrt-edgerouter-upgrade-ramips-mt7621-ubnt_edgerouter-x-initramfs-factory.tar
5ea566a6513e848a70d3ebf0d04ee171c6050f8fc41c85e9765a875ffda4e81d *openwrt-edgerouter-upgrade-ramips-mt7621-ubnt_edgerouter-x-initramfs-kernel.bin
f7ffac7569a0bac54f10fd7289149b4de381a674ddbb9665a3f5f01ca01d8d05 *openwrt-edgerouter-upgrade-ramips-mt7621-ubnt_edgerouter-x-squashfs-sysupgrade.tar

```

# Output Ubiquiti EdgeRouter X-SFP (ER-X-SFP)
```
ll bin/targets/ramips/mt7621/
total 9196
drwxr-xr-x 3 rein rein    4096 Oct  9 15:58 ./
drwxr-xr-x 3 rein rein    4096 Oct  9 15:49 ../
-rw-r--r-- 1 rein rein    1298 Oct  9 15:49 config.seed
-rw-r--r-- 1 rein rein    2371 Oct  9 15:58 openwrt-edgerouter-upgrade-ramips-mt7621-device-ubnt-edgerouter-x-sfp.manifest
-rw-r--r-- 1 rein rein 3112960 Oct  9 15:58 openwrt-edgerouter-upgrade-ramips-mt7621-ubnt_edgerouter-x-sfp-initramfs-factory.tar
-rw-r--r-- 1 rein rein 3099135 Oct  9 15:58 openwrt-edgerouter-upgrade-ramips-mt7621-ubnt_edgerouter-x-sfp-initramfs-kernel.bin
-rw-r--r-- 1 rein rein 3174595 Oct  9 15:58 openwrt-edgerouter-upgrade-ramips-mt7621-ubnt_edgerouter-x-sfp-squashfs-sysupgrade.tar
drwxr-xr-x 2 rein rein    4096 Oct  9 15:58 packages/
-rw-r--r-- 1 rein rein     677 Oct  9 15:58 sha256sums

cat sha256sums
e0d19115d51a84961074223115aa722cea98580b39eef1d57747c37263e8bf4c *config.seed
c7165019de07b7cd3fbaca1341678fabd9f279fd00b44489c5dafcb71cf3cad2 *openwrt-edgerouter-upgrade-ramips-mt7621-device-ubnt-edgerouter-x-sfp.manifest
4c8af40206a85c0246ea97c67464207c75b8aa36e718a334ed6c3317f4a84ed2 *openwrt-edgerouter-upgrade-ramips-mt7621-ubnt_edgerouter-x-sfp-initramfs-factory.tar
d2910af5dcaf4c0efcb6560f72715d84e1f060237c44a9b37895480cb39e9bd2 *openwrt-edgerouter-upgrade-ramips-mt7621-ubnt_edgerouter-x-sfp-initramfs-kernel.bin
d76ee0b794fe77d3ad65b8fa13853c20cbb85f67711587d763be5bb43bf8e48b *openwrt-edgerouter-upgrade-ramips-mt7621-ubnt_edgerouter-x-sfp-squashfs-sysupgrade.tar



```

