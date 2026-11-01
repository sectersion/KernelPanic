# notes

## reading through t113-s3 datasheet
 - booting from eMMC OR sd card, i should pick one
 - ive decided on lvsd single link lcd 11366x768 @ 60fps, //TODO read the spec for lvds
 - USB0 is dual-role, meaning it can either be a port or an input
 - USB1 is just a host, so USB0 will probably be input.
 - there are a lot of UART channels, so I will probably feature a UART debug console

### reference power design
![hello blind people](images/image.png)

4 layer board, In2 is 3v3

### constraints

| Symbol | Parameter | Min | Max | Unit |
| :--- | :--- | :--- | :--- | :--- |
| AVCC | Power Supply for Analog Part | -0.3 | 2.16 | V |
| HPVDD | Headphone Power | -0.3 | 2.16 | V |
| VCC-IO0 | Digital GPIO 0 Power | -0.3 | 3.96 | V |
| VCC-IO1 | Digital GPIO 1 Power | -0.3 | 3.96 | V |
| VCC-IO2 | Digital GPIO 2 Power | -0.3 | 3.96 | V |
| VDD-IO | Power Supply for 3.3 V Digital Pad | -0.3 | 3.96 | V |
| VCC-RTC | Power Supply for RTC | -0.3 | 3.96 | V |
| VCC-PLL | Power Supply for System PLL | -0.3 | 2.16 | V |
| VCC-MIPI | Power Supply for MIPI | -0.3 | 2.16 | V |
| VCC-TVOUT | Power Supply for TVOUT | -0.3 | 3.96 | V |
| VCC-TVIN | Power Supply for TVIN | -0.3 | 3.96 | V |
| VCC-USB0 | Power Supply for USB0 | -0.3 | 3.96 | V |
| VCC-DRAM0 | Power Supply for DRAM Core | -0.3 | 2.16 | V |
| VCC-DRAM1 | Power Supply for DRAM IO and PLL | -0.3 | 2.16 | V |
| VDD-CPU | Power Supply for CPU core system | -0.3 | 1.4 | V |
| VDD-SYS | Power Supply for System | -0.3 | 1.3 | V |
| LDO-IN | Internal LDOVR Input Voltage | -0.3 | 3.96 | V |
| LDOA-OUT | Internal LDOA Output Voltage for Analog Device and Core | -0.3 | 2.16 | V |
| LDOD-OUT | Internal LDOD Output Voltage | -0.3 | 2.16 | V |
| Tstg | Storage Temperature | -65 | 150 | °C |
| Tj | Working Junction Temperature | -40 | 125 | °C |
| Vesd | Electrostatic Discharge Voltage (Human Body Model / HBM) | -2000 | 2000 | V |
| Vesd | Electrostatic Discharge Voltage (Charged Device Model / CDM) | -250 | 250 | V |

### recommended operating conditions


| Symbol | Parameter | Min | Typ | Max | Unit |
| :--- | :--- | :--- | :--- | :--- | :--- |
| Ta | Ambient Operating Temperature (when VCC-DRAM0/1 is supplied by external power, and a heatsink is added for SoC) | -25 | - | 85 | °C |
| | Ambient Operating Temperature (when VCC-DRAM0/1 is supplied by external power) | -25 | - | 75 | °C |
| Tj | Working Junction Temperature Range | -25 | - | 125 | °C |
| AVCC | Power Supply for Analog Part | 1.782 | 1.8 | 1.818 | V |
| HPVCC | Headphone Power | 1.764 | 1.8 | 1.836 | V |
| VCC-PD | Digital GPIO D Power <br> - 1.8 V voltage <br> - 3.3 V voltage | <br>1.62 <br>2.97 | <br>1.8 <br>3.3 | <br>1.98 <br>3.63 | V |
| VCC-PE | Digital GPIO E Power <br> - 1.8 V voltage <br> - 2.5 V voltage <br> - 3.3 V voltage | <br>1.62 <br>2.25 <br>2.97 | <br>1.8 <br>2.5 <br>3.3 | <br>1.98 <br>2.75 <br>3.63 | V |
| VCC-PG | Digital GPIO G Power <br> - 1.8 V voltage <br> - 3.3 V voltage | <br>1.62 <br>2.97 | <br>1.8 <br>3.3 | <br>1.98 <br>3.63 | V |
| VCC-IO | Power Supply for Digital Part <br> - 3.3 V voltage | <br>2.97 | <br>3.3 | <br>3.63 | V |
| VCC-RTC | Power Supply for RTC | 1.62 | 1.8 | 3.63 | V |
| VCC-PLL | Power Supply for System PLL | 1.62 | 1.8 | 1.98 | V |
| VCC-LVDS | Power Supply for LVDS | 1.62 | 1.8 | 1.98 | V |
| VCC-TVOUT | Power Supply for TVOUT | TBD | 3.3 | TBD | V |
| VCC-TVIN | Power Supply for TVIN | TBD | 1.8 | TBD | V |
| VCC-DRAM0, VCC-DRAM1 | Power Supply for DRAM IO and DDR3 | 1.425 | 1.5 | 1.575 | V |
| VDD18-DRAM | Power Supply for DRAM Controller | 1.62 | 1.8 | 1.98 | V |
| VDD-CORE0, VDD-CORE1 | Power Supply for CPU and System | 0.81 | 0.9 | 0.99 | V |
| VDD-SYS0, VDD-SYS1, VDD-SYS2 | Power Supply for System | 0.81 | 0.9 | 0.99 | V |
| LDO-IN | Internal LDOA/B Input Voltage | 2.4 | 3.3 | 3.6 | V |
| LDOA-OUT | Internal LDOA Output Voltage for Analog Device and IO | 1.764 | 1.8 | 1.836 | V |
| LDOB-OUT | Internal LDOB Output Voltage | 1.21 <br>1.455 | 1.25 <br>1.5 | 1.29 <br>1.545 | V |
| | | 1.746 | 1.8 | 1.854 | V |

## reading AXP313A datasheet

 - will use USB-C @ 5V upto 3A
 - requires 2 5.1k resistors on CC1/CC2
 - AXP takes upto 5.5v input, so direct from USB-C to axp works

| Rail   | Voltage Output               |
|--------|------------------------------|
| DCDC1  | 3.3V                         |
| DCDC2  | 0.9V                         |
| DCDC3  | 1.5V                         |
| ALDO1  | 1.8V                         |
| DLDO1  | 3.3V OR 1.8V, needs decision |
| RTCLDO | 3.3V OR 1.8V, needs decision |
