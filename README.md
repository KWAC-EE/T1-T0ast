# T1-T0ast
Mini-ITX drop-in motherboard for the CM5.

Designed with the intention of allowing legacy Sata, PCIE, and ATX/ITX equipment to find a useful home.
Primarily selected components from TI in advance due to open documentation & domestic sourcing.
![image](https://github.com/user-attachments/assets/e5d51cef-71c1-4696-b5b3-43449a18482a)



**2/12/2025**
![image](https://github.com/user-attachments/assets/8a8849f6-b487-4b72-ac72-12003d1c16be)
![image](https://github.com/user-attachments/assets/b4a303cb-13a9-4137-b3a5-ad3f853d8359)

First rev arrived and I've started preliminary testing. So far, I'm able to confirm that direct passthrough IO (LAN, HDMI, SD Card) function as intended.

HDMI1 (which had a redriver IC) does not function directly, and i'm not sure if its my config file or if i screwed up the icrcuit. Either way, i might omit the redriver from rev x2.

Sadly, my SATA bridges lack the requisite Eeprom to store the firmware required to initialize, so that segement could not be tested. Separate prototypes for this segment are inbound.

The USB Hub chosen for Rev x1 claims to be USB2, but that turns out to be on the low end of the spec (12Mbps) and I wasn't able to get it to populate.

My power scheme (24pin ATX + 4-pin) seems to be working as intended. The momentary switch is superceded by a relay to keep the PSU active once the CM is powered on. At this time, Safe powe-down operation can only be completed from the desktop. Currently powering via Pico-PSU & 12VDC supply for bench setup, but larger PCIE cards/ Sata drives will likely require increased wattage and stability of a full supply.

I've been testing with a CM4 and Radxa-CM5 while i wait for my pi Cm5 (on backorder) so behavior might change when I'm able to switch to the real deal.

**3/7/2025**
Rev X2 updates based on sample testing have been laid out. Development kits for 7x USB2 hub are inbound. SATA bridges have been corrected based on feedback from TI.

Due to differences in startup behavior between rPi & Radxa CM5, hardwired delay circuits have been added to latching relays, and power button/reset have been made discrete.

Previous assembly process called out ENIG for PCB; will switch to HASL to save cost

**4/25/2025**
Rev X3 samples received and modified during testing.
![image](https://github.com/user-attachments/assets/bbf9a99b-a4e9-4688-adc5-708db7c5b113)
![image](https://github.com/user-attachments/assets/47d5ecf5-0433-4e56-b066-cf6551c1b3db)

All IO EXCEPT for the M.2 & Sata behave as intended.

TUSB9261 GRST circuit needed to be modified to allow for passive reset operation during boot. Firmware for these chips also needed to be applied externally to their requisiste SPI flash modules with a clip programmer, as TI no longer supports the flashing utility in Linux.

The TUSB8020 Hub used to drive the USB-SATA bridges is having issues. lsusb shows internal PID 8027 enumerated, but not the 8025 segment. For some reason, this means the downstream ports aren't initialised by the system.
I can't tell at the moment if this is due to a hardware fault in the pinout of the chip or the pi, or if theres too much interference from running the DP/DM traces under nearby AC caps.

The Sata bridge used for the M.2 slot initialises just fine, but I can't seem to get it to recognise an A+M key sata drive. Maybe something in the 3.3V power scheme? I'll shunt over the resettable fuse to see if it is just dropping too much voltage.

Tested several simple PCIe devices in the x16 slot like a SATA expansion and USB-C expansion card, and both are recognised by lspci. Both seem to function as intended, but thats at PCIe gen 2 speeds (haven't modified the config to try Gen 3).
A low end graphics (GT210) card i dropped in wasn't recognised by lspci, so I can't vouch for that kind of implementation just yet.

Feels like i'm nearing the finish line. In any case, the X3 does most of what i need for work purposes, so I may spend some time away from development.
