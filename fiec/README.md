
# Implementation of the Commodore Fast Serial bus using a VIA6522


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

## Test programs

All test programs starting with "dos" include the actual driver code
in the binary itself.

The test programs with "esr" work the same way, but they include the code where receiving is done
using two external 74LS164 shift register ICs (one for data, one for bit count)

The test programs expect the serial drive to have unit #9

Here is the list:

* dostst 	- show status
* dosi0		- initialize a drive (send "I0" command)
* dosdir	- show the directory (in an efficient way)
* doscat	- show the directory the BASIC4 way, with lots of UNTALK/TALK between bytes
* dosval	- validate a drive (send "V0" command)
* dosfmt	- format a drive (send "N0:...,xx" command)
* dosmkdata	- create a 64k file with pseudo random data
* dosvfdata	- verify the 64k file with pseudo random data
* dosscr	- scratch the 64k data file

