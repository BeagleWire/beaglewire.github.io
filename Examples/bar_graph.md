---
sort: 4
---

# Bar Graph

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

### Flash the FPGA with bar_graph bitstream 
```
cd examples/bar_graph

# If the fpga tools present on BBB
make

# Else scp the .bin file in Beaglewire/examples/bar_graph/

make load
```

### Running bar_graph script for transferring the memory words from ARM to FPGA (LEDs)

- Before running the bar graph script ensure that bridge libs are compiled.
```
cd Beaglewire/bridge_lib/
make
```
- Once the bridge libs are compiled:
```
cd Beaglewire/examples/bar_graph
chmod +x bar_graph.sh
sudo ./bar_graph.sh
```

### TO DO
- [ ]  Generalize C code with the help of memmap