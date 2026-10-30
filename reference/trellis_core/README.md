# Trellis Core

This repo serves as a minimum hardware design for the Allwinner T113-S4 (part
number sometimes referred to as T113M4020). This is pin-compatible with the
T113-S3, but with 256MB of RAM in-package. Although generally software-agnostic,
this repo is designed to be used with [Nerves](https://nerves-project.org) and
[nerves_system_trellis](https://github.com/protolux-electronics/nerves_system_trellis).

![A screenshot of the PCB 3D rendering](assets/pcb_render.png)

> [!NOTE]
> This hardware design has been extracted out of several proven designs that I
> have previously ordered and tested. While the schematic should be sufficient
> to minimally bring up Linux on this board, the routing and layout as is
> available in this repo has not been tested. You will almost certainly need to
> change it for your application. See the suggestions below.

Want to view the schematic and layout in your browser? Paste this repo's URL
into [KiCanvas](https://kicanvas.org/) by
[@theacodes](https://github.com/theacodes).

## Features

- Allwinner T113-S4 processor
- Flash storage - up to 8GB
- USB-C (power and data)

## Getting Started

[KiCad](https://kicad.org) version 9 or later is required to modify these
designs. To get started, clone this repo and open `trellis_core.kicad_pro` (you
may rename all `trellis_core.kicad_*` files first, if desired). The schematic
already includes:

- DC/DC converters for 3.3V and 0.9V
- 24MHz and 32.768kHz crystals for clocks
- Decoupling capacitors, strapping resistors
- SD-compatible flash storage
- Reset and FEL mode buttons
- UART debug console (UART4)

To add your required peripherals, connect the appropriate signals in the
`CPU IO` sheet. You can see the GPIO pin multiplexing capabilities for the chip
on page 30 of the
[T113 datasheet](https://linux-sunxi.org/images/7/73/T113-s3_datasheet_v1.6.pdf)

## Routing Example

The routing shown in the example board was done rather quickly to showcase the
basics of routing a PCB, while also showing best practices. It uses pretty
standard and inexpensive processes - including 0.2mm minimum trace size, 0.3mm
via drill diameter, and 0402 components. Part numbers have been included in the
metadata for each component, and parts were chosen to maximize the use of basic
and preferred components with JLC assembly.

The board uses a standard 4-layer stackup, with the following layers:

| Layer  | Type         |
| ------ | ------------ |
| Top    | Signal + GND |
| In1    | GND Plane    |
| In2    | 3.3V Plane   |
| Bottom | Signal + GND |

For the USB trace, 90 Ohm impedance is required. The trace width and spacing was
calculated for the JLC04161H-7628 stackup from JLCPCB, using the
[JLCPCB impedance calculator](https://jlcpcb.com/pcb-impedance-calculator).
Different manufacturers or stackups may require different settings - please
double check before ordering.

Finally, this design only populates components on the top side to reduce
assembly complexity and cost.

### Suggestions

As mentioned above, this exact design has not been ordered and tested
(derivatives have been, though). If you use this as the template for your
project, I suggest making the following changes to the routing:

- Use wider traces and vias for power connections: 0.9V, 1.5V, 1.8V.
- Any capacitor or resistor in the first row of components around the chip
  should stay close to the chip. Do not move them very far away, as they are
  required for the stability of the design
- If using double sided assembly, feel free to place the decoupling capacitors
  on the reverse side. This will simplify some of the routing and fanout of
  signals
- Keep the SDIO signals for the flash chip relatively short. Right now, they are
  between 20mm and 25mm in length. They should be approximately the same
  length - no more than 10mm different. SDIO is a relatively high speed signal
  of about 50MHz in this configuration. Trace length tuning has been used on
  some of the traces to meet this requirement (don't stress too much though -
  50MHz is pretty forgiving)
- Add stitching vias. Currently, the GND planes are not very well connected, as
  I did not place stitching vias. Via on traces for high speed signals should
  have a nearby GND via, and empty space could be filled with a grid via pattern

## License

Copyright 2025 by Protolux Electronics SARL-S

Trellis Core hardware design files are released under the CERN Open Hardware
Licence, Permissive (CERN-OHL-P). See the LICENSE file for the complete legal
text. In summary:

- You are free to use, study, modify, and distribute the design files for any
  purpose, including personal, educational, or commercial.
- You may make and sell products based on this design without any obligation to
  release your own modifications.
- If you do choose to share modified design files, you must:
  - Retain existing copyright and license notices.
  - Include a short note describing your modifications (what was changed and
    when).
  - Provide a copy of the CERN-OHL-P license alongside your files.

This is a permissive open hardware license, and is similar in spirit to MIT or
BSD licenses in software. It encourages reuse, even in commercial contexts,
while preserving proper attribution.

> [!NOTE]
> Some 3D component models and other assets in this repository are provided
> under their original authors’ licenses. Refer to the individual files or their
> directories for details.

Disclaimer: The Trellis Core design is provided "as is", without any warranty or
guarantee of performance. Use, manufacture, or modification of this hardware is
entirely at your own risk.

## Trademark and Logo Use

The Protolux Electronics name and logo are trademarks of Protolux Electronics
SARL-S.

These marks are not licensed under the CERN-OHL-P license. You may not use the
Protolux Electronics logo or trademarks on any modified versions of this
hardware, on derivative products, or in any way that suggests endorsement or
origin from Protolux Electronics, unless you have received prior written
permission.

## Projects Using this Design

Feel free to open a PR to add your project here! These are some existing
projects that are based off of this design:

| Name                                                                        | Author                           | Description                                                                                                                           |
| --------------------------------------------------------------------------- | -------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------- |
| [Wisteria](https://github.com/protolux-electronics/wisteria_hardware)       | Protolux Electronics/Gus Workman | Wisteria is the hardware for the 2025 Goatmire e-ink name badge. Read more about it [here](https://protolux.io/projects/goatmire2025) |
| [Trellis Dev Kit](https://github.com/protolux-electronics/trellis_hardware) | Protolux Electronics/Gus Workman | WIP! Check back soon :)                                                                                                               |
| [Nerves Getting Started Hardware Kit]()                                     | Protolux Electronics/Gus Workman | WIP! Check back soon :)                                                                                                               |
| Your board here                                                             | You                              | Share your design - open a PR!                                                                                                        |

## Designed by Protolux Electronics

Need help with your next embedded project? Interested in a customized T113-based
Nerves board? We're Protolux Electronics, a Luxembourg-based consultancy that
provides hardware and embedded software design services, as well as
manufacturing support. We specialize in embedded solutions using Nerves and
Elixir. Check us out at [https://protolux.io](https://protolux.io)
