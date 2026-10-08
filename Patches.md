# Patches 
This version of OpenWrt is based on OpenWrt V18.06.9 with unnecessary functions stripped out to fit within the 3MiB limit. The Luci web gui is included so the installation and upgrade to newer versions can be done all via Graphical User Interface.  
**Automatic conversion to 6MiB partition is build in!**

# 3MiB limit
In order not to exceed the 3MiB limit, this version is based on OpenWrt V18.06.9. Any higher version results in exceeding the 3MiB limit when Luci is included. The goal is this build is to make the installation of OpenWrt as easy as possible without the need for a USB to serial adapter. SSH is supported by this build, however it requires your SSH client to support for older Hostkey algorithm RSA/sha1.

See [Build Instructions.md](#Build%20Instructions.md)

# Automatic conversion to 6MiB partition
This is achieved by updating the dts for Ubiquiti EdgeRouter X (ER-X) and Ubiquiti EdgeRouter X-SFP (ER-X-SFP).

#### `target/linux/ramips/dts/UBNT-ER-e50.dtsi`:  
The kernel1 and kernel2 partition are replaced by a single kernel partition with double size.
```
        partition@140000 {
                label = "kernel";
                reg = <0x140000 0x600000>;
        };
```


#### `package/base-files/files/bin/config_generate`:
Added compat_version='2.0' to the config for system. This is needed since the sysupgrade validates that the new firmware (OpenWrt v24 and higher) is compatible with the current version.     
`set system.@system[-1].compat_version='2.0'`

#### `target/linux/ramips/dts/UBNT-ERX-SFP.dts`:  
Replaced 'UBNT-ERX-SFP' with 'Ubiquiti EdgeRouter X SFP'.  
Replaced 'ubiquiti,edgerouterx-sfp' with 'ubnt,edgerouter-x-sfp'.  
Replaced 'ubnt-erx' with 'ubnt,edgerouter-x'.  
This is needed to match with the naming in recent OpenWrt versions.  

#### `ubiquiti,edgerouterx-sfp/ubnt,edgerouter-x-sfp`:
Replaced 'Device/ubnt-erx' with 'Device/ubnt_edgerouter-x'.  
Replaced 'TARGET_DEVICES += ubnt-erx' with 'TARGET_DEVICES += ubnt_edgerouter-x'.  
This is needed to match with the naming in recent OpenWrt versions.

#### `target/linux/ramips/base-files/etc/board.d/03_gpio_switches`:
Replaced 'ubnt-erx' with 'ubnt,edgerouter-x'.  
This is needed to match with the naming in recent OpenWrt versions.

#### `target/linux/ramips/base-files/etc/board.d/02_network`:
Replaced 'ubnt-erx' with 'ubnt,edgerouter-x'.  
This is needed to match with the naming in recent OpenWrt versions.

#### `target/linux/ramips/base-files/lib/upgrade/platform.sh`:
Replaced 'ubnt-erx' with 'ubnt,edgerouter-x'.  
This is needed to match with the naming in recent OpenWrt versions.

#### `target/linux/ramips/dts/UBNT-ER-e50.dtsi`:
Replaced 'ubnt-erx' with 'ubnt,edgerouter-x'.  
This is needed to match with the naming in recent OpenWrt versions.

### To support the newer sysupgrade format of OpenWrt v24 and higher the following files are borrowed from OpenWrt v25.12.
```
cp ./openwrt-v25.12/package/base-files/files/lib/upgrade/common.sh ./openwrt-v18.06.9/package/base-files/files/lib/upgrade/common.sh
cp ./openwrt-v25.12/package/base-files/files/lib/upgrade/do_stage2 ./openwrt-v18.06.9/package/base-files/files/lib/upgrade/do_stage2
cp ./openwrt-v25.12/package/base-files/files/lib/upgrade/fwtool.sh ./openwrt-v18.06.9/package/base-files/files/lib/upgrade/fwtool.sh
cp ./openwrt-v25.12/package/base-files/files/lib/upgrade/nand.sh ./openwrt-v18.06.9/package/base-files/files/lib/upgrade/nand.sh
cp ./openwrt-v25.12/package/base-files/files/lib/upgrade/stage2 ./openwrt-v18.06.9/package/base-files/files/lib/upgrade/stage2
cp ./openwrt-v25.12/package/base-files/files/sbin/sysupgrade ./openwrt-v18.06.9/package/base-files/files/sbin/sysupgrade
  
cp ./openwrt-v25.12/target/linux/ramips/mt7621/base-files/lib/upgrade/platform.sh ./openwrt-v18.06.9/target/linux/ramips/base-files/lib/upgrade/platform.sh

cp ./openwrt-v25.12/target/linux/ramips/mt7621/base-files/lib/upgrade/ubnt.sh ./openwrt-v18.06.9/target/linux/ramips/base-files/lib/upgrade/ubnt.sh 

cp ./openwrt-v25.12/package/base-files/files/lib/upgrade/tar.sh ./openwrt-v18.06.9/target/linux/ramips/base-files/lib/upgrade/tar.sh

cp ./openwrt-v25.12/package/base-files/files/usr/libexec/validate_firmware_image ./openwrt-v18.06.9/package/base-files/files/usr/libexec/validate_firmware_image
```
After coping the following files are updated:
#### `package/base-files/files/sbin/sysupgrade`:
Replaced 'export SAVE_CONFIG=1' with 'export SAVE_CONFIG=0'
Since the new firmware config (OpenWrt v24 and higher) is not compatible with v18, keeping the config files results in a improper network configuration that does not allow to connect. Therefore keeping the config files is disabled to prevent misconfiguration. Fixing this misconfiguration would require TFTP recovery via the reset button, which is rather complicated!

#### `package/base-files/files/usr/libexec/validate_firmware_image`:
Replaced 'ALLOW_BACKUP=1' with 'ALLOW_BACKUP=0'. There is no need to back up the old configuration.