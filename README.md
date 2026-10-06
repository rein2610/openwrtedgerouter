
      _______                     ________        __
    |       |.-----.-----.-----.|  |  |  |.----.|  |_
    |   -   ||  _  |  -__|     ||  |  |  ||   _||   _|
    |_______||   __|_____|__|__||________||__|  |____|
              |__| W I R E L E S S   F R E E D O M
    -----------------------------------------------------

# Installing OpenWrt on Ubiquiti EdgeRouter X (ER-X) and Ubiquiti EdgeRouter X-SFP (ER-X-SFP)

## Preface
Installing OpenWrt on Ubiquiti EdgeRouter X (ER-X-SFP) can be rather complicated. 
Ubiquiti EdgeRouter X (ER-X-SFP) comes with EdgeOS installed. Although EdgeOS support the installation of third-party firmware, there is a limitation of 3MiB for the kernel image.  

In order to install OpenWrt, the two kernel partitions of EdgeOS with each a size of 3MiB need to be merged into one 6MiB partition.  

This specific version of OpenWrt for Ubiquiti EdgeRouter X makes it easy to install recent OpenWrt versions without the need of serial converter cable hooked into the serial device pins.  

This version of OpenWrt is based on OpenWrt V18.06.9 with unnecessary functions stripped to fit within the 3MiB limit. The Luci web gui is included so the installation and upgrade to newer versions can be done all via Graphical User Interface.

## Quick installation guide
### Prerequisite
- EdgeOS installed
- `openwrt-edgerouter-upgrade-ramips-mt7621-ubnt_edgerouter-x-sfp-initramfs-factory.tar` downloaded on your computer
- `openwrt-24.10.8-ramips-mt7621-ubnt_edgerouter-x-sfp-squashfs-sysupgrade.bin` downloaded on your computer

### Installation
- Connect your computer to eth0 port of the Ubiquiti EdgeRouter X (ER-X-SFP) and make sure your computer's ip address in the 192.168.1.x/24 subnet.
- Go to https://192.168.1.1/#Dashboard
- Log in with ubnt/ubnt
- Go to System / Upgrade System Image
- Upload `openwrt-edgerouter-upgrade-ramips-mt7621-ubnt_edgerouter-x-sfp-initramfs-factory.tar`
- Reboot

 
Wait two minutes

- Connect your computer to eth1 port
- Go to http://192.168.1.1/cgi-bin/luci//admin/
- Log in (without password)
- Go to System / Backup / Flash Firmware
- Flash new firmware image: `openwrt-24.10.8-ramips-mt7621-ubnt_edgerouter-x-sfp-squashfs-sysupgrade.bin` (or higher)
- Keep settings is enabled by default and will not have any effect, so no need to uncheck.
- Verify the checksum and file size listed, compare them with the original file to ensure data integrity.
- Proceed

Wait a minute

- Go to http://192.168.1.1/cgi-bin/luci//admin/

That’s all. No serial cable needed. No ssh login needed. :tada:

> [!NOTE]
> Example above is for EdgeRouter X-SFP (ER-X-SFP), same applies for EdgeRouter X (ER-X). Just remove -sfp from the filenames.

