# GPIO 

Features:
  - dodatkowe 54 piny I/O
  - kazdy pin jest mapowany w pamieci
  - easy to implement

Introduction:
This project allows to use FPGA (iCE40) pins on BeagleWire board from the level of userspace on linux on ARM.
The projects consists of a GPIO controller and a mapping component. Eight pins can be connected to each GPIO 
controller. There are a total of 48 pins I/O. 
```
  +-----------+     +------------------------------------------------------------------+
  |  ARM      |     |    FPGA iCE40                          +------------+            |
  |          -------->                             +-------->| GPIO PORT 1|            |
  | memory    GPMC                                 | +------>|            |<-------------->  PMOD1
  |          <--------      fpga registers         | | +-----|            |            |
  |           |     |       are mapped in arm      | | |     +------------+            |     
  |           |     |       +-----------------+    | | |           *       <-------------->  PMOD2
  +-----------+     |       | direct register |----+ | |                               |     
                    |       | output register |----|-+ |           *       <-------------->  PMOD3
                    |       | input register  |<---|-|-+                               |     
                    |       +-----------------+    | | |           *       <-------------->  PMOD4
                    |                              | | |     +------------+            |
                    |                              +-|-|---->| GPIO PORT 5|            | 
                    |                              | +-|---->|            |<-------------->  GROVE
                    |                              | | +-----|            |            |
                    |                              | | |     +------------+            |
                    |                              | | |     +------------+            |
                    |                              +-|-|---->| GPIO PORT 6|            |
                    |                                +-|---->|            |<-------------->  [SWs - BTNs - LEDs]
                    |                                  +-----|            |            |
                    |                                        +------------+            |
                    +------------------------------------------------------------------+
```

Background:

**About GPMC:**
Registers of GPIO controllers are mapped in ARM memory using GPMC bus. More information regarding logic mapping can be found at gpmc-ice40-manual.txt.

**Direct register:**
It is used to set GPIO direction. Each pin can be either input (0) or output (1).

**Output register:**
It is used to set the state on output. The pin has to be on out enable mode (dir set to 1).

**Input register:**
It is used to read the state on input. The pin has to be on output disabled mode (dir set to 0).

BeagleWire Pins Alignment:
```
 Bit |  Port  | Pin | iCE40 Pin |
-----+--------+-----+-----------+
  0  |  Pmod1 |  0  |    38
  1  |  Pmod1 |  1  |    37
  2  |  Pmod1 |  2  |    41
  3  |  Pmod1 |  3  |    39
  4  |  Pmod1 |  4  |    43
  5  |  Pmod1 |  5  |    42
  6  |  Pmod1 |  6  |    45
  7  |  Pmod1 |  7  |    44
-----+--------+-----+-----------+  
  8  |  Pmod2 |  0  |    48
  9  |  Pmod2 |  1  |    47
  10 |  Pmod2 |  2  |    52
  11 |  Pmod2 |  3  |    49
  12 |  Pmod2 |  4  |    56
  13 |  Pmod2 |  5  |    55
  14 |  Pmod2 |  6  |    62
  15 |  Pmod2 |  7  |    60
-----+--------+-----+-----------+  
  16 |  Pmod3 |  0  |    117
  17 |  Pmod3 |  1  |    110
  18 |  Pmod3 |  2  |    112
  19 |  Pmod3 |  3  |    113
  20 |  Pmod3 |  4  |    116
  21 |  Pmod3 |  5  |    115
  22 |  Pmod3 |  6  |    129
  23 |  Pmod3 |  7  |    130
-----+--------+-----+-----------+  
  24 |  Pmod4 |  0  |    4
  25 |  Pmod4 |  1  |    7
  26 |  Pmod4 |  2  |    8
  27 |  Pmod4 |  3  |    9
  28 |  Pmod4 |  4  |    10
  29 |  Pmod4 |  5  |    15
  30 |  Pmod4 |  6  |    11
  31 |  Pmod4 |  7  |    12
-----+--------+-----+-----------+  
  24 | grove1 |  0  |    73
  25 | grove1 |  1  |    74
  26 | grove2 |  0  |    75
  27 | grove2 |  1  |    76
  28 | grove3 |  0  |    104
  29 | grove3 |  1  |    102
  30 | grove4 |  0  |    106
  31 | grove4 |  1  |    105
-----+--------+-----+-----------+  
  24 |  Led   |  0  |    28
  25 |  Led   |  1  |    29
  26 |  Led   |  2  |    31
  27 |  Led   |  3  |    32
  28 |  Btn   |  0  |    25
  29 |  Btn   |  1  |    26
  30 |  Sw    |  0  |    33
  31 |  Sw    |  1  |    34
-----+--------+-----+-----------+  
```

Memory map:
```
 offset    | register name                                                             |
-----------+---------------------------------------------------------------------------+
    0x00   | Direct register                                                           |
-----------+---------------------------------------------------------------------------+
    0x08   | Output register                                                           |
-----------+---------------------------------------------------------------------------+
    0x10   | Input register                                                            |
-----------+---------------------------------------------------------------------------+
```
Direct register (0x00)
```
   bit  | default |      | destination                             |
--------+---------+------+-----------------------------------------+
  63-0  |    0    |  R/W | Port direction    1 - Output 0 - Input  |
--------+---------+------+-----------------------------------------+
```

Output register (0x08)
```
   bit  | default |      | destination          |
--------+---------+------+----------------------+
  63-0  |  Output |  R/W | Set GPIOs value      |
--------+---------+------+----------------------+
```
Input register (0x04)
```
   bit  | default |      | destination          |
--------+---------+------+----------------------+
  63-0  |    0    |  RO  | State on GPIOs line  |
--------+---------+------+----------------------+
```

GPIO Component Port Descriptions:
```
  Port         | Width | Mode | Interface    | Description                                 |
---------------+-------+------+--------------+---------------------------------------------+
  io           |   8   |IN/OUT| User logic   | GPIO port                                   |
---------------+-------+------+--------------+---------------------------------------------+
  dir          |   8   |  IN  | User logic   | Port direction    1 - Output 0 - Input      |
---------------+-------+------+--------------+---------------------------------------------+
  Input        |   8   |  IN  | User logic   | State on GPIOs line                         |
---------------+-------+------+--------------+---------------------------------------------+
  Output       |   8   |  OUT | User logic   | Set GPIOs value                             |
---------------+-------+------+--------------+---------------------------------------------+
```

Component owners:
Patryk Mezydlo <mezydlo.p@gmail.com>
