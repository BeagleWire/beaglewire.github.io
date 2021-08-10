# SPI 

Features:
  - Verilog source code of a Serial Peripheral Interface (SPI) master component
  - Configurable data width
  - Configurable speed
  - Selectable clock polarity and clock phase

Introduction:
This project provides a ready SPI Master controller written in Verilog, which is intended for FPGAs. This project is developed with the use of IceStorm Toolchain. It contains SPI Master controller and GPMC component - thanks to that SPI controller is mapped in ARM memory. This controller can be used as any other controller located in in the silicon of your processor.
```
  +-----------+     +-------------------------------------------------------------------------+
  |  arm      |     |    fpga ice40                                                           |
  |          -------->                                +-------------------+                   |
  | memory    gpmc                             +----->|                   |                   |
  |          <--------    fpga registers       |+-----|    SPI MASTER   -------------------------> SCLK
  |           |     |     are mapped in arm    ||     |    COMPONENT      |                   |
  |           |     |     +-----------------+  ||     |                 -------------------------> MOSI
  +-----------+     |     | setup register  |--+|     |                   |                   |
                    |     | status register |<--+     |                 <------------------------- MISO
                    |     | tx data register|-------->|                   |                   |
                    |     | rx data register|<--------|                   |                   |
                    |     | clk div register|-+       +-------------------+                   |
                    | +---| cs register     | |                         ^                     |
                    | |   +-----------------+ |                         |                     |
                    | |                       |                         |                     |
                    | |                       |        +-------------+  |                     |
                    | |                       +------->|clock divider|--+                     |
                    | |                                +-------------+                        |
                    | |                                                                       |
                    | +---------------------------------------------------------------------------> CS
                    |                                                                         |
                    +-------------------------------------------------------------------------+
```
Background:

About SPI:
The Serial Peripheral Interface bus (SPI) is a synchronous serial communication interface.
An SPI communication is a full-duplex, using four wires. 

Reset:
Reset is asynchronous and active after turning on the system. Reset should be turned off by clearing bit on the first position in setting register (0x00).

Clock:
Controller has a frequency divider. The value of frequency divider can be changed anytime. Its value is located in setting register on position 15-10.The source of clock for SPI is 20MHz. By setting divider to 2 you will receive 10MHz etc.

Bits per word:
Controller supports various word lengths, ranging from 2 to 32 bits. On the beginning of each transaction the number of bits is latched. From the number of bits 1 has to be deducted and written into setting register on 9:5 position.

Polarity and Phase:
The enable pin latches in the standard logic values of cpol and cpha at the start of each transaction. This allows communication with individual slaves using independent SPI modes. If all slaves require the same mode, cpol and cpha can simply be tied to the corresponding logic levels.

Transactions:
	1. Set the transaction settings,
	2. Load data into transmit buffer (0x4),
	3. Set start bit,
	4. Clear start bit (at this stage the word is transmitted),
	5. Wait until the new_data bit is set,
	6. Data is ready to receive,
	7. Back to step 2,

Memory map:
```
 Offset   | Register name                       |
----------+-------------------------------------+
    0x0   | setup register                      |
----------+-------------------------------------+
    0x2   | status register                     |
----------+-------------------------------------+
    0x4   | tranceive register                  |
----------+-------------------------------------+
    0x8   | receive register                    |
----------+-------------------------------------+
```
Setup register (0x00)
```
   bit  | default |      | destination          |
--------+---------+------+----------------------+
    0   |    1    |  R/W | Reset controller     |
--------+---------+------+----------------------+
    1   |    0    |  R/W | Start sending data   |
--------+---------+------+----------------------+
    2   |    0    |  R/W | Cpol setting bit     |
--------+---------+------+----------------------+
    3   |    0    |  R/W | Cpha setting bit     |
--------+---------+------+----------------------+
    4   |    0    |  R/W | Chip select (CS) bit |
--------+---------+------+----------------------+
   9-5  |    0    |  R/W | Bits per word        |
--------+---------+-----------------------------+
  15-10 |    0    |  R/W | Clock divider        |
--------+---------+------+----------------------+
```
Status register (0x02)
```
   bit  | default |      | destination          |
--------+---------+------+----------------------+
    0   |    0    |  RO  | Busy                 |
--------+---------------------------------------+
    1   |    0    |  RO  | New data for receive |
--------+---------+------+----------------------+
```
Tranceive data register (0x04)
```
   bit  | default |      | destination          |
--------+---------+------+----------------------+
  31-0  |    0    |  R/W | Data to tranceive    |
--------+---------+------+----------------------+
```
Receive data register (0x08)
```
   bit  | default |      | destination          |
--------+---------+------+----------------------+
  31-0  |    0    |  R/W | Data to receive      |
--------+---------+------+----------------------+
```
SPI Component Port Descriptions:
```
   Port        | Width | Mode | Interface    | Description                                 |
---------------+-------+------+--------------+---------------------------------------------+
  clk          |   1   |  IN  | User logic   | System clock                                |
---------------+-------+------+--------------+---------------------------------------------+
  rst          |   1   |  IN  | User logic   | Asynchronous active low reset               |
---------------+-------+------+--------------+---------------------------------------------+
  sck          |   1   |  BUF | Slave device | SPI clock                                   |
---------------+-------+------+--------------+---------------------------------------------+
  miso         |   1   |  IN  | Slave device | Master in, slave out data line              |
---------------+-------+------+--------------+---------------------------------------------+
  mosi         |   1   |  OUT | Slave device | Master out, slave in data line              |
---------------+-------+------+--------------+---------------------------------------------+
  cpol         |   1   |  IN  | User logic   | SPI clock polarity setting                  |
---------------+-------+------+--------------+---------------------------------------------+
  cpha         |   1   |  IN  | User logic   | SPI clock phase setting                     |
---------------+-------+------+--------------+---------------------------------------------+
  bits_per_word|   5   |  IN  | User logic   | Numer of bits per word                      |
---------------+-------+------+--------------+---------------------------------------------+
  div          |   6   |  IN  | User logic   | Clock divider                               |
---------------+-------+------+--------------+---------------------------------------------+
  data_in      |   32  |  IN  | User logic   | Data to transmit                            |
---------------+-------+------+--------------+---------------------------------------------+
  data_out     |   32  |  OUT | User logic   | Data received from target slave             |
---------------+-------+------+--------------+---------------------------------------------+
  busy         |   1   |  OUT | User logic   | Busy signal                                 |
---------------+-------+------+--------------+---------------------------------------------+
  start        |   1   |  IN  | User logic   | Start sequence signal,                      |
               |       |      |              |set to 1 and then to 0 to enable transaction |
---------------+-------+------+--------------+---------------------------------------------+
  new_data     |   1   |  OUT | User logic   | Data ready signal                           |
---------------+-------+------+--------------+---------------------------------------------+
```
Component owners:
Patryk Mezydlo <mezydlo.p@gmail.com>
