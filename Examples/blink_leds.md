---
sort: 3
---

## Blink LEDs


- Example Directory: [blink_leds](https://github.com/BeagleWire/BeagleWire/tree/master/examples/blink_leds)
- The LEDs are blink via simple counter logic

### Flash the FPGA with blink_leds bitstream 
```
cd examples/blink_leds

# If the fpga tools present on BBB
make

# Else scp the .bin file in Beaglewire/examples/blink_leds

make load
```
