# GPMC 

Features:


Introduction:
The GPMC component provides an easy to implement solution to exchange data with ARM processor. In this way, 
one can easily map the logic in processor’s memory. This is possible thanks to the GPMC pins located outside 
of BeagleBoard boards. The project has been designed for use with BeagleWire. The bus width amounts to 16 lines, 
while the clock frequency is 25MHz, allowing an efficient exchange of data. The GPMC controller can be configured 
from DTS file containing information on timing settings, method of sending data as well as settings of mapped 
memory location. The manual describes how to use shared memory on FPGA. The module has been written in verilog 
with the use of IceStorm Toolchains. 

```
  +-----------+     +-------------------------------------------------------------------------+
  |  ARM      |     |    FPGA iCE40                                                           |
  |          <------->        
  | memory   GPMC Data      
  |          <--------   
  |           |     |   
  |           |     |   
  +-----------+     | 
                    |     
                    |   
                    |  
                    |
                    |
                    | 
                    | 
                    |
                    |
                    |
                    |
                    |
                    |                                                                         
                    +-------------------------------------------------------------------------+
```

Background:

About GPMC:
Registers of GPIO controllers are mapped in ARM memory using GPMC bus. More information regarding 
logic mapping can be found at gpmc-ice40-manual.txt.
