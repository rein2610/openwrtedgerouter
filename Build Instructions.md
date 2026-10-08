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

# Output
```
rein@ubuntu31:~/openwrtedgerouter$ ll bin/targets/ramips/mt7621/
total 9208
drwxr-xr-x 3 rein rein    4096 Oct  7 22:11 ./
drwxr-xr-x 3 rein rein    4096 Oct  7 22:07 ../
-rw-r--r-- 1 rein rein    1276 Oct  7 22:07 config.seed
-rw-r--r-- 1 rein rein    2416 Oct  7 22:11 openwrt-edgerouter-upgrade-ramips-mt7621-device-ubnt-edgerouter-x-sfp.manifest
-rw-r--r-- 1 rein rein 3112960 Oct  7 22:11 openwrt-edgerouter-upgrade-ramips-mt7621-ubnt_edgerouter-x-sfp-initramfs-factory.tar
-rw-r--r-- 1 rein rein 3102398 Oct  7 22:11 openwrt-edgerouter-upgrade-ramips-mt7621-ubnt_edgerouter-x-sfp-initramfs-kernel.bin
-rw-r--r-- 1 rein rein 3184839 Oct  7 22:11 openwrt-edgerouter-upgrade-ramips-mt7621-ubnt_edgerouter-x-sfp-squashfs-sysupgrade.tar
drwxr-xr-x 2 rein rein    4096 Oct  7 22:11 packages/
-rw-r--r-- 1 rein rein     677 Oct  7 22:11 sha256sums
```

