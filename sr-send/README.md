
# VIA shift register sending behaviour

## When switching to output via ACR

Deduction: when reading or writing to the SR, the bit counter is reset to 8 anyway (if it is not already in progress), no matter if sending or receiving.
E.g. when receiving, we read the last byte received and thus set the bit counter. IFF we do this while the shifting is already in progress, 
the bit counter is not reset, but we either read partially shifted in data, or even overwrite those bits that have not yet shifted out.

However, when switching to output, as the bit counter is 8, it will then directly start shifting the current SR value on the next shift condition.
(this has been tested with T2 as timer source, see below).

If T2 is the shift condition, T2L underflow will trigger the shift.

Tests tx2, tx3, and tx4 show this behaviour with varying positions when the first T2L underflow happens.

## T2

Shifting out (and assumingly in) with T2 puts the timer into a specific mode only used for shifting.

In the normal mode, T2 has a low byte counter T2LC (read only) and a low byte latch T2LL (write only), but only a high byte counter T2H.
The purpose of this is that you can set the target counter value into the low byte latch, then write the high byte into the counter and at
this time trigger the transfer of the T2LL into the T2LC. This basically enables a synchronized 16 bit transfer into the timer.

On an underflow, however, T2 cannot get back to the original counter value, as there is no T2 high byte latch that could have stored this
value. Instead the counter has been set directly, and then counted down. Therefore, the T2 counter, on underflow, just keeps counting down
to -1, -2, -3, ... and so on.

(test tx1 shows this)

## T2 as shift clock source

When being used as shift clock, T2 needs a way to restore the original timer value, so it can count down for every half-bit.
As T2 only has a low byte latch T2LL, only the low byte is used to count clocks for the shift register!
I.e. when counting down, on every underflow of T2L only(!) T2LC is loaded from T2LL, while T2H is just decremented by one. That leads 
to counting values like:

	$0104, $0103, $0102, $0101, $0100, $00ff, $0004, $0003, $0002, $0001, $0000, $ffff, $ff04, $ff03, $ff02, $ff01, $ff00, $feff, $fe04, ...

On every underflow of T2L (only!) the CB1 shift clock is toggled. 

Note that this sequence is not documented as such. The Rockwell datasheet and timing measurements show that each underflow takes "N+2" cycles.
For example if T2L is set to 4, each underflow is 6 cycles, a single shifted bit thus takes 12 cycles.
The values depicted above - esp. the information that counting goes down to $ff - are taken as from the Timer 1 one-shot mode timing 
diagram of the rockwell datasheet, with the exception of the adapted high byte value.

## Critical timing when start shifting

A critical timing situation appears when the SR bit counter is set (due to a previous read/write of SR), and SR is set from another mode
into mode 101 - shift out under T2 (maybe also 001 shift in under T2).
The shift register then immediately starts shifting out at the first T2L underflow, sending whatever data currently is in the shift register. 
Setting a new value during this time overwrites part of the shifted byte and thus mangles the data(!) 
Test tx4 shows this

The easiest way to overcome this problem is to write the next byte to be sent to the SR before setting the SR to output via T2.
(Note: SR must be enabled, e.g. when set to shift in via external clock, to actually set the bit counter. If SR is disabled
when writing, no shift will occur)

As before setting the SR mode the T2 timer is in one-shot mode, it may be at any state of a 16 bit counter, more specifically,
T2L may be in any of 256 states. This may introduce a random delay of up to 256-N cycles after setting the SR to output before shifting
starts. 

Specific timer control may have to be applied to reduce the potential delay. However, just resetting the timer while running
may change the bit/shift interval. Thus the timer needs to be reset before setting to output, ensuring it overruns before setting to output
and being somewhere in the $fx range, then setting to output and resetting the timer again to the low value it should use.

tx5 shows resetting the timer and writing the data before changing mode to output, tx6 adds resetting the timer after setting to output

## Additional notes

The focus on this investigation was sending out data via SR using T2 as a source.

The same may happen when shifting in under T2 control, as well as shifting in or out under phi2 control.



