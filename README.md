
# Installing OpenWrt on TP-LINK WR1043ND V3

### Introduction

This tutorial will guide you through the installation of OpenWrt + OpenVSwitch 2.7 with SNMP support on TP-LINK WR1043ND V3. It is essential to notice that the files provided in this repository are for version 3 of the router and that there is no guarantee that they will work on other versions of it. Also, keep in mind that the change of firmware is a violation of the warranty and there is a risk of bricking the equipment while doing so. The files used in this tutorial were obtained at trustworthy sources ([OpenWrt](https://openwrt.org/)/[TP-LINK](https://www.tp-link.com/)). If you desire, you can download the files directly from their sources.
Because LEDE project is not maintained anymore, I'll keep the files here for safety.

### Steps

1. Firstly connect the RJ45 cable directly on one of the ethernet ports and access the routers configuration page by going on ```192.168.1.1```.
The default credentials are ```admin\admin```.

2. Through the menu, go to ```System Tools -> Firmware Upgrade``` and select the file: ```lede-17.01.4-ar71xx-generic-tl-wr1043nd-v3-squashfs-factory.bin```. Confirm and wait. After a while, the router will reboot.


3. Now with OpenWrt (the project was rebranded as LEDE for a while) installed, reaccess its configurations on the same IP address ```192.168.1.1)```.  You will notice the LuCi interface. The credentials are ```admin``` with a blank password.

4. By going on ```System -> Software```, you can verify that among the packages that are already installed, OpenVSwich and SNMP are not present. Because of this, we must patch the router's SO with another image.

5. Going on menu System -> Backup / Flash Firmware on ```Flash new firmware image``` option, select the patch file ```openwrt-ar71xx-generic-tl-wr1043nd-v3-squashfs-sysupgrade.bin```. After a few seconds, the router will reboot. Notice that it is not possible to access the router configuration through the browser. This happens because after patching the system, some packages were lost in the process, including LuCi.

6. Now it is necessary to access the router through SSH. Navigate to the terminal and type ```ssh -Y root@192.168.1.1```. A screen similar will appear at your terminal. Connect the cable on WAN port and update the list of available packages by typing ```opkg update```;

7. Then, install LuCi by typing ```opkg install luci```. Also type ```opkg instsall snmpd``` to install SNPMd.

Now your router is operating with OpenWrt, including OpenVSwitch 2.7.0, SNMP e LuCi. To see a list of installed packages, type ```opg list-installed```. This information can also be seen on the browser, like step 4.
At this moment, your router is working as an ordinary router.

### Reinstalling default firmware 

If you wish to go back to TP-LINK's original firmware, do like the step 5, but select the file `plink-1043nd-v3-stripped.bin`. This file is a variation of the original found on [TP-Link website](https://www.tp-link.com/br/download/TL-WR1043ND_V3.html#Firmware), but without boot menu functionality, that could cause conflict while OpenWrt is present. You can download the original file on tp link and remove the boot menu by yourself, just as it is done [here](https://wiki.openwrt.org/toh/tp-link/tl-wr1043nd#back_to_original_firmware).

### Common problems

> I cannot access the router using SSH. The IP address is not found.

Configure your network interface manually with the IP ```192.168.1.2``` and try again. Remember that the cable must be connected in one of the LAN ports, not in the WAN.

> I still cannot access, even with the static IP address.

Reboot the router with secure mode. When the system light starts blinking, press the WPS/reset button on the router until it starts blinking faster. 
Now try again.

### Contact

Robertson Lima - [robertsonlima91@gmail.com](mailto:robertsonlima91@gmail.com)
