# UART 

features:
  - verilog source code of a universal asynchronous receiver/transmitter (uart)
  - full duplex
  - configurable baud rate
  - configurable data width
  - no flow control
  - configurable parity bit (none/even/odd)
  - confirurable stop bit
  - fifo queue

introduction:
```
  +-----------+     +-------------------------------------------------------------------------+
  |  arm      |     |    fpga ice40                                                           |
  |          -------->                                                                        |
  | memory    gpmc                                                                            |
  |          <--------      fpga registers                                                    |
  |           |     |       are mapped in arm                                                 |
  |           |     |       +----------------+                          +------------------+  |
  +-----------+     |       | uart tx setting|<------------------------>| uart tx component|---->  tx
                    |       | tx fifo regs   |------+               +-->|                  |  |
                    |       | uart rx setting|<---+ |   +-------+   |   +------------------+  |
                    |       | rx fifo regs   |<-+ | +-->|fifo tx|---+                         |
                    |       +----------------+  | |     +-------+                             |
                    |                           | |                                           |
                    |                           | |                     +------------------+  |
                    |                           | +-------------------->| uart rx component|<----  rx
                    |                           |                   +---|                  |  |
                    |                           |       +-------+   |   +------------------+  |
                    |                           +-------|fifo rx|<--+                         |
                    |                                   +-------+                             |
                    +-------------------------------------------------------------------------+
```

Memory map:

```
 offset    | register name                                                             |
-----------+---------------------------------------------------------------------------+
    0x00   | uart tx setup                                                             |
-----------+---------------------------------------------------------------------------+
    0x02   | uart tx clock divider                                                     |
-----------+---------------------------------------------------------------------------+
    0x04   | uart tx fifo setup                                                        |
-----------+---------------------------------------------------------------------------+
    0x06   | uart tx fifo data input                                                   |
-----------+---------------------------------------------------------------------------+
    0x08   | uart rx setup                                                             |
-----------+---------------------------------------------------------------------------+
    0x0A   | uart rx clock divider                                                     |
-----------+---------------------------------------------------------------------------+
    0x0C   | uart rx fifo setup                                                        |
-----------+---------------------------------------------------------------------------+
    0x0E   | uart rx fifo data output                                                  |
-----------+---------------------------------------------------------------------------+
```

UART TX setup register (0x00)
```
   bit  |  default |      |  destination                                               |
--------+----------+------+------------------------------------------------------------+
    0   |     1    |  R/W | reset uart and fifo clr this bit to enable controller      |
--------+----------+------+------------------------------------------------------------+
    1   |     0    |  R/W | enable send                                                |
--------+----------+------+------------------------------------------------------------+
    2   |     0    |  RO  | busy                                                       |
--------+----------+------+------------------------------------------------------------+
    3   |     0    |  R/W | parity enable                                              |
--------+----------+------+------------------------------------------------------------+
    4   |     0    |  R/W | parity type 0 = evan 1 = odd                               |
--------+----------+------+------------------------------------------------------------+
    5   |     0    |  R/W | two stop bit enable                                        |
--------+----------+------+------------------------------------------------------------+
   10-6 |     7    |  R/W | bits per word (numer of bits per word - 1)                 |
--------+----------+------+------------------------------------------------------------+
```

UART TX clock divider (0x02)
```
   bit  | default  |      | destination                                                |
--------+----------+------+------------------------------------------------------------+
  15-0  |     0    | R/W  | clock divider                                              |
--------+----------+------+------------------------------------------------------------+

UART TX FIFO setup register (0x04)

   bit  | default  |      | destination                                                |
--------+----------+------+------------------------------------------------------------+
    0   |     0    | R/W  | generate write to fifo signal (set to 1 and then clr bit)  |
--------+----------+------+------------------------------------------------------------+
    1   |     0    | RO   | fifo empty                                                 |
--------+----------+------+------------------------------------------------------------+
    2   |     0    | RO   | fifo full                                                  |
--------+----------+------+------------------------------------------------------------+
   8-3  |     0    | RO   | fifo counter (number of words in buffer)                   |
--------+----------+------+------------------------------------------------------------+
```

UART TX data input (0x06)
```
   bit  | default  |      | destination                                                |
--------+----------+------+------------------------------------------------------------+
  15-0  |     0    | R/W  | data to send                                               |
--------+----------+------+------------------------------------------------------------+
```
UART RX setup register (0x08)
```
   bit  |  default |      |  destination                                               |
--------+----------+------+------------------------------------------------------------+
    0   |     0    |  R/W | reset uart and fifo set this bit to enable controller      |
--------+----------+------+------------------------------------------------------------+
    1   |     0    |  R/W | enable receive                                             |
--------+----------+------+------------------------------------------------------------+
    2   |     0    |  RO  | busy                                                       |
--------+----------+------+------------------------------------------------------------+
    3   |     0    |  R/W | parity enable                                              |
--------+----------+------+------------------------------------------------------------+
    4   |     0    |  R/W | parity type 0 = evan 1 = odd                               |
--------+----------+------+------------------------------------------------------------+
    5   |     0    |  R/W | two stop bit enable                                        |
--------+----------+------+------------------------------------------------------------+
    6   |     0    |  R/W | frame error                                                |
--------+----------+------+------------------------------------------------------------+
    7   |     0    |  RO  | new data to read                                           |
--------+----------+------+------------------------------------------------------------+
   12-8 |     7    |  R/W | bits per word (numer of bits per word - 1)                 |
--------+----------+------+------------------------------------------------------------+
```

UART RX clock divider (0x0A)
```
   bit  | default  |      | destination                                                |
--------+----------+------+------------------------------------------------------------+
  15-0  |     0    | R/W  | Clock divider                                              |
--------+----------+------+------------------------------------------------------------+
```

UART RX FIFO setup register (0x0C)
```
   bit  | default  |      | destination                                                |
--------+----------+------+------------------------------------------------------------+
    0   |     0    | R/W  | generate write to fifo signal (set to 1 and then clr bit)  |
--------+----------+------+------------------------------------------------------------+
    1   |     0    | RO   | fifo empty                                                 |
--------+----------+------+------------------------------------------------------------+
    2   |     0    | RO   | fifo full                                                  |
--------+----------+------+------------------------------------------------------------+
   8-3  |     0    | RO   | fifo counter (number of words in buffer)                   |
--------+----------+------+------------------------------------------------------------+
```

UART RX data output (0x0E)
```
   bit  | default  |      | destination                                                |
--------+----------+------+------------------------------------------------------------+
  15-0  |     0    | R/W  | data to send                                               |
--------+----------+------+------------------------------------------------------------+
```

UART Transmitter Component Port Descriptions:
```
   Port           | Width | Mode | Interface          | Description                                 |
------------------+-------+------+--------------------+---------------------------------------------+
  clk             |   1   |  IN  | User logic         | System clock                                |
------------------+-------+------+--------------------+---------------------------------------------+
  rst             |   1   |  IN  | User logic         | Asynchronous active low reset               |
------------------+-------+------+--------------------+---------------------------------------------+
  tx              |   1   |  out | Uart correspondent | SPI clock                                   |
------------------+-------+------+--------------------+---------------------------------------------+
  two_stop_bit    |   1   |  IN  | User logic         | 0 - one stop biy 1 - two stop bits          |
------------------+-------+------+--------------------+---------------------------------------------+
  parity_en       |   1   |  IN  | User logic         | Parity bit                                  |
------------------+-------+------+--------------------+---------------------------------------------+
  parity_evan_odd |   1   |  IN  | User logic         | Type of parity 0-evan 1-odd                 |
------------------+-------+------+--------------------+---------------------------------------------+
  bits_per_word   |   5   |  IN  | User logic         | Numer of bits per word                      |
------------------+-------+------+--------------------+---------------------------------------------+
  clk_div         |   6   |  IN  | User logic         | Clock divider                               |
------------------+-------+------+--------------------+---------------------------------------------+
  data_in         |   15  |  IN  | User logic         | Data to transmit                            |
------------------+-------+------+--------------------+---------------------------------------------+
  busy            |   1   |  OUT | User logic         | Busy signal                                 |
------------------+-------+------+--------------------+---------------------------------------------+
  wr_en           |   1   |  IN  | User logic         | Start sequence signal,                      |
                  |       |      |                    | set to 1 and then to 0 to enable transaction|
------------------+-------+------+--------------------+---------------------------------------------+
  new_data        |   1   |  OUT | User logic         | Data ready signal                           |
------------------+-------+------+--------------------+---------------------------------------------+
```

Component owners:
Patryk Mezydlo <mezydlo.p@gmail.com>