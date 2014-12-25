# Part 2: Hardware Security
## Simple Attacks
- Remove hard drive from machine
- Boot from USB key and retrieve data or change passwords
- Trivial, and possibly broken, countermeasures
	- Change BIOS settings to not allow external boot sources (USB, CDs, etc.)
	- Protect BIOS by password (to counter change settings)
		- use BIOS reset jumpers to reset settings
		- remove battery on mainboard to reset BIOS settings
	- Disk encryption
		- Attacker still can wipe disk
		- Key can be found using invasive attacks (e.g. frozen DRAM)
		- Key can be found using less invasive attacks (e.g. FireWire DMA)
	- Hide data

## ColdBoot
-  The belief that DRAM looses its content right after power is gone is wrong
-  Cooling down DRAM enlarges time window to extract data
-  Disk encryption key is stored in RAM while computer is locked
	1. Expose RAM chips
	2. Remove power from machine
	3. Cool down RAM
	4. Plug RAM into attacker controlled machine
	5. Read out content of RAM
	6. Find disk encryption key
- Attack possible even without cooling down RAM : Boot very small binary to read out RAM content
- Countermeasures:
	- Don't keep keys in RAM (e.g. use TPM)
	- Use two-factor authentication
	- Dismount encrypted disks
	- Store key in CPU registers
	- Embed temperature sensor close to RAM

## (FireWire) DMA
- Access to RAM is controlled by CPU, and therefore by the OS. However, this can be circumvented by Direct Memory Access (DMA)
- FireWire uses DMA for high speed data transfer
- Attacker uses FireWire to access memory using DMA while PC is locked
- Not only FireWire a problem:
	- PCMCIA
	- Cardbus
	- ExpressCard
- Mitigations: remove/limit DMA access

## Current Technology
- Thunderbolt expands PCIe bus, one can use a FireWire to Thunderbolt adapter
- BadUSB
	- Changes USB device controller to be another device class (in addition to the original one)
	- Can act for example as a keyboard, network card, etc.

## Van Eck Eavesdropping
- Eavesdrop on CRT/LCD display content by detecting electromagnetic emissions
- Possible to do over distances and through walls
- Countermeasures
	- Shielding equipment/room
	- Scramble image, keeping image perceptually equal for a human but makes recorded emissions hard to reverse engineer
	- low-pass filters for fonts
	- randomize the least significant bit (LSB)

## (sp)iPhone
- Use smartphone's accelerometer sensors to decode keys pressed by a user
- phone is near keyboard on the table
- Countermeasure
	- put phone further away from keyboard
	- user other surface, wood is a very bad choice (amplifies keystrokes)
	- high typing speed
	- add noise (e.g. fan on table)

## Attack on Mobile Devices
- TouchLogger
	- Get pressed key on on-screen keyboard (smartphones) by observing smartphone's movement
	- Use smartphone's accelerometer
	- Generated reference data set of keystrokes
- Smudge Attacks
	- Use smudges (oily residue) left on screen to read out inputs (e.g. Android's gesture unlock screen)