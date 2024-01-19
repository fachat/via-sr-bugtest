
# Implementation of the Commodore Fast Serial bus using a VIA6522

Current state: when sending the "S0:PSEUDO64K" command, on the bus we see the "S" ok, but then is a bit inserted, and all following bytes are shifted by one bit

## Pinout:

PB0: FSERDIR: 1 = input, 0 = output (sending)
PB1/2: n/a
PB3: ATNOUT
PB4: CLKOUT
PB5: DATAOUT
PB6: CLKIN
PB7: DATAIN

CB1: Fast serial clock <-> SRQ
Cb2: Fast serial data <-> DATA

