# T1-T0ast
Mini-ITX drop-in motherboard for the CM5.

Designed with the intention of allowing legacy Sata, PCIE, and ATX/ITX equipment to find a useful home.
Primarily selected components from TI in advance due to open documentation & domestic sourcing.
![image](https://github.com/user-attachments/assets/81c5d3a1-226b-44c1-8c26-a72be6fd8abd)



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

**5/8/2025**
 Rev X4 changes underway
 
I've removed the manually timed reset circuits for the latching relay and Sata bridges, and made them passive. The leveling capacitors on the TUSB9261s are larger than recommended, with the intention of delaying reset until 2x the time of their host USB3 hubs.

I've also removed & replaced the TUSB8020 with an 8041 for the sata bridges, routing the spare USB3 lanes to a FP-IO header.

Per TI's recommendations, I've added AC coupling caps on BOTH the RX & TX lines between the pi and hubs. The IO board for the CM5 lacks these and seem to function ok, but in the interest of Giving myself an out, i will at least include the pads (couple replace RX caps with 0ohm resistors if deemed unnecessary)

Biffed the firmware flashing process and my flashing tool during debugging, hopefully will receive replacement chip-clips soon. Ideally, i could pre-flash the spi modules before doing a full build, but the bridges on the full board dont seem to like receiving rev 1.06 firmware binaries as much as their discrete counterparts on my dev kits. what this means is that I will need to port windows to a CM5 (WoP project has thankfully done enough for me to try this) and use the native TI Flash-burner utility to do this within the system.

but holy hell, i reran the costs for this dev build, and its over a grand for 5 boards from JLCPCB with the nonsensical tarrifs (domestics are much higher and have MOQ of 10, which i think is even more wasteful if a batch is a dud).
If you don't see an update from me after this, its because i've mothballed development until a sane adult is in charge.

**6/4/2025**
Well, here i am. Bit the bullet. one more go from me, fam

Rev X4 with some IO modifications is in flight. If this doesn't work, i entrust the next weary traveler to pick up the torch and carry on.

results to follow

**6/30/2025**
Works as well as I can reasonably hope. had to remove the AC caps to leave both RX & TX floating on the hub which drives the 2 sata bridges.

Been troubleshooting this issue so long, with tickets to both TI & The pi forums, and i cant find a solution to the USB3 enumeration issue.

I'll package and release the current hardware production files once I daily drive this model for a bit. Sent a sample for review, so if that brought you here, welcome!

Don't take my design as gospel, but it is all at least 98% functional, from the power regulation to the IO expanders and adapters. If you have a project which could use part of this design, adherent to the open-source nature of the project as it is, have at it.

If you need design resources, or you're interested in seeing which hardware can be made compatible with this platform, check out my other repositories.
