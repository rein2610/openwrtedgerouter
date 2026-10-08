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
Target Profile (Ubiquiti EdgeRouter X-SFP)  

Under Global Build settings  
- [ ] Enable IPv6 support in packages 
- Under Kernel build options
    - [ ] Compile the kernel with debug filesystem enabled  
    - [ ] Compile the kernel with symbol table information (NEW)  
    - [ ] Compile the kernel with debug information  
    - [ ] Enable process core dump support (NEW)  
    - [ ] Enable IPv6 multicast routing (NEW)  
- [*] Strip unnecessary exports from the kernel image (NEW)
- [*] Strip unnecessary functions from libraries (NEW)

Under LUCI
- Under 1. Collections
  - <*> luci................... LuCI interface with Uhttpd as Webserver (default)
- Under 2. Modules  --->
    - [*] Minify Lua sources

Under Base system
 - < > opkg................................................ opkg package manager  ----

Under Network  
 - Under SSH  
   - <*> openssh-sftp-server.................................. OpenSSH SFTP server



Alternative: use the provided `.config`

```
git restore .config
```
