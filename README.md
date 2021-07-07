---
sort: 1
---

# BeagleWire - FPGA development cape for the BeagleBone

- The BeagleWire is an FPGA development platform that has been designed for use with BeagleBone boards. BeagleWire is a cape on which there is an FPGA device - Lattice iCE40HX.
- The Lattice iCE40 is a family of FPGAs with a minimalistic architecture and very regular structure, designed for low-cost, high-volume consumer and system applications. 
- The significance of FPGAs is continuously increasing, as they are more and more often used to support ARM processors. BeagleWire does not require external tools (JTAG) and the whole software is Open Source.
- iCE40 is an energy saving device, allowing to work with small batteries. FPGA cape allows easy and low cost start for beginners who would like to take their first steps in working with FPGAs. 
- The developed software will be an easy and, at the same time, efficient tool for communication with FPGA. At this point FPGA will be able to meet the requirements of even more advanced applications. 
- The BeagleWire creates a powerful and versatile digital cape for users to create their imaginative digital designs.

## Table of Contents
- [BeagleWire](#beaglewire---fpga-development-cape-for-the-beaglebone)
  - [BeagleWire features](#beagleWire-features)
  - [BeagleWire Peripherals](#beagleWire-peripherals)
  - [Software\Driver support](#softwaredriver-support)
  - [Getting BBB Ready for BeagleWire](#getting-bbb-ready-for-beaglewire)
  - [Authors](#authors)
  - [Resources](#resources)

### BeagleWire features

- FPGA: Lattice iCE40HX4K - TQFP 144 Package
- GPMC port access from the BeagleBone
- SPI programming port from the BeagleBone
- 4 layer PCB optimized design to support maximum performance - for high bandwidth applications
- BeagleBoard optimized - compatible with BeagleBone Black, BeagleBone Black Wireless, element14 BeagleBone Black Industrial
- Does not require external tools (JTAG)
- minimalistic architecture and very regular structure
- energy saving device allows to work with small batteries
- lower application costs
- fully open-source toolchain

### BeagleWire Peripherals

- 32 MB SDRAM
- 100Mhz external clock
- 4 LEDs
- 4 PMOD connectors
- 4 Grove connectors
- 2 user push buttons
- 2 input DIP switch

### Software\Driver support

- BeagleWire software support is still developing. A lot of useful examples and ready to use solutions can be found there. For communication between FPGA and ARM, GPMC can be used. 
- This is an easy and efficient solution. BeagleWire software repository has special components for it. You can just map your logic in BeagleBone memory. 
- A lot of examples have kernel driver e.g. SPI, GPIO. After preparing the FPGA device you can use it as a typical GPIO/SPI from user space. 
- A part of examples do not belong to kernel subsystems e.g. rotary excoder, ultrasonic ranger sensor or blink leds. In these examples, you can connect to FPGA using write/read operation for /dev/mem file.

### **Getting BBB Ready for BeagleWire**

- Detailed Steps for preparing BBB to flash BeagleWire:
[Getting BBB Ready for BeagleWire
](https://beaglewire.github.io/Blogs/Getting_BBB_Ready_for_BeagleWire.html#getting-bbb-ready-for-beaglewire)

### Resources

- [BeagleWire KiCAD Repository](https://github.com/BeagleWire/beagle-wire)
- [BeagleWire Software Repository](https://github.com/BeagleWire/BeagleWire/tree/testing)
- [BeagleWire Hackaday.io project page](https://hackaday.io/project/20989-beaglewire)
- [BeagleWire Hackster.io project page](https://www.hackster.io/46021/beaglewire-566292)
- [BeagleWire Schematic](https://github.com/BeagleWire/beagle-wire/blob/master/plots/beagle-wire.pdf)

### Authors

The project is the result of the community work and it is still under development. If you can support this project or if you have any questions, feel free to contact us.
- Michael Welling mwelling@ieee.org
- Omkar Bhilare ombhilare999@gmail.com
- Patryk Mezydlo mezydlo.p@gmail.com
