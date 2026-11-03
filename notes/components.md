# Component Choices

## How this is organized:

Subsystem > Section > Component

## Component Choices

### Power Subsystem

#### Input Section

USB-C power input:
 - 5v @ ~3A via 2x 5.1k resistors
 - Feeds global 5v rail/label
 - Will include a 1x2 2.54mm header for 5v and ground
 - Taken from trellis ref design, should add polyfuse on VBUS
 - Taken from trellis ref design, should add USB ESD protection


#### Power Path and Rail Generation

Levels needed:
 - 3.3v for most things, sensors etc
 - 5v also availible from USB
 - 1.8/1.5/0.9 availible from AXP313A

Requirements:
 - test point on each rail for easy testing + GND


#### Power sequencing

| AXP step | Rail(s) | Voltage | Feeds (T113-S3) | Rationale |
|----------|---------|---------|-----------------|-----------|
| Step 0 | RTCLDO | 1.8V (TBD) | VCC-RTC | Always-on domain, ramps no later than all others |
| Step 0 | DCDC1 | 3.3V | VCC-IO/PD/PE/PG, VCC-TVOUT, LDO-IN | VCC-IO group first |
| Step 0 | ALDO1 | 1.8V | AVCC, VCC-PLL, VCC-LVDS, VDD18-DRAM | 1.8V group with 3.3V; VCC_LVDS ramps with VCC_PD |
| Step 0 | DLDO1 | 3.3V helpers (TBD) | Pullups / helpers only, 500mA max | With step 0, not bulk |
| Step 1 (+≥2ms) | DCDC2 | 0.9V | VDD-SYS0/1/2, VDD-CORE0/1 | T1>2ms after 3.3V @90% |
| Step 1–2 | DCDC3 | 1.5V | VCC-DRAM0/1 | Stable before SDRAM init |
| Last | PWROK > SoC RESET | — | RESET release +64ms, then 24MHz starts | T2>64ms |


### Storage Subsystem

The reference design uses a ZDSD08GLGEAG 8 gigabit NAND. ([datasheet here](https://datasheet.lcsc.com/lcsc/2005251034_XTX-XTSD08GLGEAG_C558840.pdf)) I think this would probably be the most simple way to go about this, as I hate BGA and that's what eMMC would be. I may also add an SD card slot since there are multiple SPI peripheral slots. 

TLDR: SPI

Update: SD cards and eMMC both use SMHC, not SPI. Thankfully, there are 3 SMHC interface

SMHC0 will be connected to a SD card slot, SMHC1 will be connected to ZDSD08GLGEAG (NAND flash), and SMHC2 will be DNP

### Debugging

 - Test points EVERYWHERE (I learned my lesson)
 - UART debug console on UART0
 - PWR_GOOD leds
 - PWROK Test point
 - MicroSD/Storage activity led
 - 0 ohm resistors for current measurements


### Ethernet Subsystem

 on Port E!

 I'm going to go for a 100 megabit speed (which should be plenty) over RMII. I will use a LAN8742A, and include notes here:

 - Kernel manages PHY through "SMI" which means serial management interface. It's a TWI and I need to learn how this works
 - PHY requires a external crystal/clock ref
 - Takes 3.3V so that's easy enough

 | Pin Num | Pin Name | Pin Num | Pin Name |
| :---: | :--- | :---: | :--- |
| 1 | VDD2A | 13 | MDC |
| 2 | LED2/nINT/nPME/nINTSEL | 14 | nINT/REFCLKO |
| 3 | LED1/nINT/nPME/REGOFF | 15 | nRST |
| 4 | XTAL2 | 16 | TXEN |
| 5 | XTAL1/CLKIN | 17 | TXD0 |
| 6 | VDDCR | 18 | TXD1 |
| 7 | RXD1/MODE1 | 19 | VDD1A |
| 8 | RXD0/MODE0 | 20 | TXN |
| 9 | VDDIO | 21 | TXP |
| 10 | RXER/PHYAD0 | 22 | RXN |
| 11 | CRS_DV/MODE2 | 23 | RXP |
| 12 | MDIO | 24 | RBIAS |
source: from datasheet

Looking at magnetics, I think I'd rather not. So - enter the MagJack.. A beautiful piece of engineering. I found the HANRUN(Zhongshan HanRun Elec) HR911105A, and I should just be able to conenct it directly.