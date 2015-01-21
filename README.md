# MeasureFlipInterval

2015-01-21 dxjones@gmail.com

The goal of this program is to help diagnose and debug video timing issues under different hardware and software configurations.

Comments, feedback, and pull requests are welcome.

This program reliably measures the "Flip Interval",
... the time between vertical blanking events in the video refresh cycle.

It also measures the delay
between the moment the vertical blanking begins (VBL_timestamp)
and the moment the Screen('Flip',...) command returns control to the main program.
On some computers, this delay is very brief; on others it is often close to 3 msec, which is very long.

This program has been tested using:

	Psychtoolbox version 3.0.12 - Flavor: beta - Corresponds to SVN Revision 5797

and various iMac computers running Mac OSX 10.9.x and 10.10.x

