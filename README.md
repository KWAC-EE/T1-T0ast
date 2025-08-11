# T1-T0ast
Mini-ITX drop-in motherboard for the CM5.

Designed with the intention of allowing legacy Sata, PCIE, and ATX/ITX equipment to find a useful home.
Primarily selected components from TI in advance due to open documentation & domestic sourcing.

# Project design files
PCBAs are designed in Altium 22 & 25. This is a paid software platform for professionals, and viewer licenses can be requested from the developers.
Now that the design is roughly complete, I will spend some time learning how to port the design directly into KiCad or EasyEDA for more direct access to hobbyists.
Design references can also be found as PDFs for those who wish to rebuild segments of the design themselves.

# PCBA exports/ Manufacturer files
Designs have been exported as Gerbers with Pick&Place & BOM files such that they may be fully assembled by prototypers such as JLC-PCB, PCB-Way and the like.
Simply upload the zipped project export folder for the PCB, then the internal XLXS/CSV component-level assembly.
Component selection of passives was left generic to allow for flexible sourcing. ICs and other specified parts have a parameter called "LCSC PN", which can be used to preorder components from either LCSC or JLCPCB prior to placing an assembly order.
Some footprints are custom, and part number left unmatched. These have been hand-soldered inhouse or are used for debugging, and I dont recommend having a board house manage these for cost or quality reasons.

# Firmware & Software support
All Hubs are plug&play, and are meant to function with the Raspberry pi CM5 without manual driver installation.
Direct IO is passed from the CM to plug headers without modification.
USB-Audio codec has integrated drivers in Raspberry Pi OS, and Auto-installs via Windows update if accessible.
USB-SATA Bridge requires flashing to the NAND module prior to operation. Hex/bin files to do so are available in corresponding folder.

# License
Copyright Kurt Widhalm/@KWAC-EE 2025
This source describes Open Hardware and is licensed under the CERN-OHL-P v2

You may redistribute and modify this documentation and make products using it under the terms of the CERN-OHL-P v2 (https:/cern.ch/cern-ohl). This documentation is distributed WITHOUT ANY EXPRESS OR IMPLIED WARRANTY, INCLUDING OF MERCHANTABILITY, SATISFACTORY QUALITY AND FITNESS FOR A PARTICULAR PURPOSE. Please see the CERN-OHL-P v2 for applicable conditions
