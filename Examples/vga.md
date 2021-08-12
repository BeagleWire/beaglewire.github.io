---
sort: 5
---

## VGA

- VGA PMOD is connected to the beaglewire P1, P4 Pmods.
- pinmap can be found in pcf file.
- It is connected upside down to port3 and port4.

### Flash the FPGA with VGA example bitstream 

```
cd Beaglewire/examples/vga

# If the fpga tools present on BBB
make

# Else scp the .bin file in Beaglewire/examples/vga
# In host computer go to Beaglewire/examples/v
# make
# Command to send it to FPGA: 
# scp vga.bin debian@192.168.6.2:/home/debian/Beaglewire/examples/vga

# Loading SPI flash after FPGA reset, it will be boot up on SPI.
make load_spi

# Reset the FPGA for running bitsream (RST Button on BeagleWire)
```

### Demo:

- Demo Video of VGA can be found here: [Imgur](https://imgur.com/sAeCMZ2)