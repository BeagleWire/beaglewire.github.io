# PWM (verilog)

Features:
  - Verilog source code of a PWM generator component
  - Configurable duty cycle resolution
  - Configurable number of outputs/phases
  - Configurable PWM frequency
  - Modulation around the center of the pulse
  - Configurable signal polarity

Introduction:
The project provides a complete PWM (Pulse Width Modulation) module written in verilog, which is 
intended for use with FPGAs. It has been created with the use of IceStorm toolchains. 

```
  +-----------+     +------------------------------------------------------+
  |  ARM      |     |    FPGA iCE40                                        |
  |          -------->                                                     |           
  | memory    GPMC                                                         |
  |          <--------      fpga registers                                 |
  |           |     |       are mapped in arm                              |
  |           |     |       +-----------------+       +----------------+   |
  +-----------+     |       | Setup           |------>|  PWM component |   |
                    |       | Period          |------>|                |   |
                    |       | Duty Cycle      |------>|                |   |
                    |       +-----------------+       |                |-------> PWM pins
                    |                                 |                |   |
                    |       +--------------+          |                |   |
                    |       | PLL   200MHz |--------->|                |   |
                    |       +--------------+          +----------------+   |          
                    |                                                      |
                    +------------------------------------------------------+
```

Background:

About GPMC:
The controller is mapped in ARM memory with the use of GPMC bus. Thanks to that, after connecting a Linux driver or a simple application, it is possible to control frequency, duty cycle and polarization of PWM signal.

Duty Cycle:
In this register, a signal’s duty cycle can be set in nano seconds:
PWM_duty_counter = Duty_ns / Clock_period (200MHz = 20ns)

Period: 
The register is used to set the frequency of PWM signal. It is necessary to calculate the counter’s value:  PWM_counter = Period_ns / Clock_period (200MHz = 20ns).

Clock:
The clock’s source for PWM controller is 200MHz. Thanks to that, the generated signal is more accurate.

Enable:
Setting this bit activates generation of PWM signal.

Reset:
Reset is asynchrononous and active after turning on the system. Reset should be turned off by clearing bit on the first position in setup register (0x00).

Polarity:
Setting polarity bit (position 3) causes the inversion of PWM signal.

Reset is asynchronous and active after turning on the system. Reset should be turned off by clearing bit on the first position in setting register (0x00).

Memory map:
```
 offset    | register name                                                             |
-----------+---------------------------------------------------------------------------+
    0x00   | Setup register                                                            |
-----------+---------------------------------------------------------------------------+
    0x02   | Period register                                                           |
-----------+---------------------------------------------------------------------------+
    0x06   | Duty Cycle register                                                       |
-----------+---------------------------------------------------------------------------+
```

Setup register (0x00)
```
   bit  | default |      | destination                             |
--------+---------+------+-----------------------------------------+
    0   |    1    |  R/W | Reset bit                               |
--------+---------+------+-----------------------------------------+
    1   |    0    |  R/W | Enable bit                              |
--------+---------+------+-----------------------------------------+
    2   |    0    |  R/W | Polarity bit                            |
--------+---------+------+-----------------------------------------+
```

Period register (0x08)
```
   bit  | default |      | destination          |
--------+---------+------+----------------------+
  31-0  |  Output |  R/W | Period register      |
--------+---------+------+----------------------+
```

Duty Cycle register (0x04)
```
   bit  | default |      | destination          |
--------+---------+------+----------------------+
  31-0  |    0    |  RO  | Duty Cycle register  |
--------+---------+------+----------------------+
```

PWM Component Port Descriptions:
```
  Port         | Width | Mode | Interface    | Description                                 |
---------------+-------+------+--------------+---------------------------------------------+
  en           |   1   |  IN  | User logic   | Enable bit (0 PWM controller is active)     |
---------------+-------+------+--------------+---------------------------------------------+
  period       |   3   |  IN  | User logic   | Period                                      |
---------------+-------+------+--------------+---------------------------------------------+
  duty_cycle   |   8   |  IN  | User logic   | Duty Cycle                                  |
---------------+-------+------+--------------+---------------------------------------------+
  polarity     |   8   |  IN  | User logic   | Polarity                                    |
---------------+-------+------+--------------+---------------------------------------------+
  clk          |   8   |  IN  | User logic   | Clock source                                |
---------------+-------+------+--------------+---------------------------------------------+
  out          |   8   |  OUT | Load         | Output PWM signals                          |
---------------+-------+------+--------------+---------------------------------------------+
```
Component owners:
Patryk Mezydlo <mezydlo.p@gmail.com>
