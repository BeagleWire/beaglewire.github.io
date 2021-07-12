---
sort: 3
---

# BeagleWire Examples

## File Structure:

```
.
├── arm_blink_leds 
├── bar_graph
├── blink_leds
├── gpio
├── i2c
├── lcd
├── lcd_game
├── Makefile
├── Makefile.beaglewire
├── pwm
├── README.md
├── sdram
├── spi
├── stepper_motor
└── uart
```

## 1) [Arm Bink Leds](https://github.com/BeagleWire/BeagleWire/tree/master/examples/arm_blink_leds) 

- The LEDs are blink via ARM memory.
- GPMC has been used between ARM and FPGA for memory Transfer
- Further the GPMC protocol has been converted to Wishbone protocol
- [Detailed Steps](https://beaglewire.github.io/Examples/arm_blink_leds.html)

## 2) [Bink Leds](https://github.com/BeagleWire/BeagleWire/tree/master/examples/blink_leds)

- The LEDs are blink via simple counter logic
- [Detailed Steps](https://beaglewire.github.io/Examples/arm_blink_leds.html)

## 3) [Bar Graph](https://github.com/BeagleWire/BeagleWire/tree/master/examples/bar_graph)

- This Example is the template for wishbone intercon
- The testcase consist of:
    1. Wishbone Slave 1: PMOD 3 (Bar Graph 0)
    2. Wishbone Slave 2: PMOD 4 (Bar Graph 1)
- Address Map
```
        Wishbone Memmap
        Slave 0:  0x0000
        Slave 1:  0x0040
```
- Wishbone intercon automatically routes signals and data to the appropriate slave depending on the memmap
- [Detailed Steps](https://beaglewire.github.io/Examples/bar_graph.html)