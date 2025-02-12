# T1-T0ast
Mini-ITX drop-in motherboard for the CM5.

Designed with the intention of allowing legacy Sata, PCIE, and ATX/ITX equipment to find a useful home.
Primarily selected components from TI in advance due to open documentation & domestic sourcing.
![image](https://github.com/user-attachments/assets/ceaf5b70-2d87-4256-9097-2fc8a0a3a318)

**2/12/2024**
![image](https://github.com/user-attachments/assets/8a8849f6-b487-4b72-ac72-12003d1c16be)
![image](https://github.com/user-attachments/assets/b4a303cb-13a9-4137-b3a5-ad3f853d8359)

First rev arrived and I've started preliminary testing. So far, I'm able to confirm that direct passthrough IO (LAN, HDMI, SD Card) function as intended.
HDMI1 (which had a redriver IC) does not function directly, and i'm not sure if its my config file or if i screwed up the icrcuit. Either way, i might omit the redriver from rev x2.
Sadly, my SATA bridges lack the requisite Eeprom to store the firmware required to initialize, so that segement could not be tested. Separate prototypes for this segment are inbound.
The USB Hub chosen for Rev x1 claims to be USB2, but that turns out to be on the low end of the spec (12Mbps) and I wasn't able to get it to populate.
My power scheme (24pin ATX + 4-pin) seems to be working as intended. The momentary switch is superceded by a relay to keep the PSU active once the CM is powered on. At this time, Safe powe-down operation can only be completed from the desktop. Currently powering via Pico-PSU & 12VDC supply for bench setup, but larger PCIE cards/ Sata drives will likely require increased wattage and stability of a full supply.
I've been testing with a CM4 and Radxa-CM5 while i wait for my pi Cm5 (on backorder) so behavior might change when I'm able to switch to the real deal.
