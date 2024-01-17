# via-sr-bugtest

These files implement test cases where you connect either 
two Commodore PET or two Commodore VIC-20 using userport connection.

Then run "m?rcv" on the receiver side, and then m?send on the sender side.

The sender will send 64k of data over the serial shift register of the VIA
and the receiver will receive it and at the end print out statistics 
of data received and errors detected.

## PET

The PET files are mpsnd and mprcv. 

Load them using DLOAD or LOAD, and you start them with SYS4096

## VIC-20

The VIC-20 files are mvsnd and mvrcv. 

Load them using LOAD"mvsnd",8,1 and you start them with SYS4111


