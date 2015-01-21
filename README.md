# MeasureFlipInterval

2015-01-21 dxjones@gmail.com

The goal of this program is to help diagnose and debug video timing issues under different hardware and software configurations.

At startup Psychtoolbox tries to measure the flip interval empirically, but sometimes fails.
This initial "Sync Test" can be skipped by passing an extra parameter.

	MeasureFlipInterval(1);
	
Comments, feedback, and pull requests are welcome.

This program reliably measures the "Flip Interval",
... the time between vertical blanking events in the video refresh cycle.

It also measures the delay between
the moment the vertical blanking begins (VBL_timestamp)
and the moment the Screen('Flip',...) command returns control to the main program
and executes the GetSecs command.
On some computers, this delay is very brief; on others it is often close to 3 msec, which is very long.

This program has been tested using:

	Psychtoolbox version 3.0.12 - Flavor: beta - Corresponds to SVN Revision 5797

and various iMac computers running Mac OSX 10.9.x and 10.10.x

## Results

The results of running this program on several different iMac computers is summarized in Results.pdf

## Psychtoolbox Video Timing Bugs

1. VBL Sync Failure at startup

On some Mac computers, Psychtoolbox fails the VBL Sync Test on startup,
but this program MeasureFlipInterval(1) will succeed, with no missed Flips.

Therefore, the Psychtoolbox VBL Sync Failure should be fixed by following the example code here.

2. Strange 3 millisecond delay following vertical blanking

On some Mac computers,
there is a 3 millisecond delay between the vertical blanking event (VBL_timestamp)
and the moment the Screen('Flip',...) command returns control to the main program and executes the GetSecs command.
This delay seems to occur before the beam position query (based on the beam position returned).

Since 3 milliseconds is a large fraction of the Flip Interval (16.67 msec), this suggests there is a bug in the code.
