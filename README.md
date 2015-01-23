# MeasureFlipInterval

Author:  David Jones, 2015-01-21 dxjones@gmail.com

The goal of this program is to help diagnose, debug, and fix
some of the video timing issues in Psychtoolbox
under different hardware and software configurations,
especially on recent iMacs.

Comments, feedback, and pull requests are welcome.

## Measuring the Flip Interval

At startup, as part of the first Screen('OpenWindow',...) command,
Psychtoolbox tries to measure the flip interval empirically, but sometimes fails.
This initial "Sync Test" can be skipped by passing an extra parameter.

	MeasureFlipInterval(1);
	

This program reliably measures two things:

1. The "Flip Interval", which is the time between vertical blanking events in the video refresh cycle.
It does this by calling the Screen('Flip',...) command 500 times, and then calculating summary statistics.

2. This program also measures the extra delay between the moment the vertical blanking begins (VBLTimestamp)
and the moment just before the Screen('Flip',...) command returns control to the main program (FlipTimestamp).
This delay seems to occur during the execution of the Screen('Flip',...) command, before the beam position query.
On some computers, this delay is brief (about 0.5 msec); on others it is often close to 3 msec, which is a very long delay.

This program has been tested using:

	Psychtoolbox version 3.0.12 - Flavor: beta - Corresponds to SVN Revision 5797

and various iMac computers running Mac OSX 10.9.x and 10.10.x

## Results

The results of running this program on several different iMac computers is summarized in Results.pdf

If you run this program on a different iMac model,
please cut-and-paste your results from the Matlab command window
and send them to dxjones@gmail.com.
I will add your results to Results.pdf

## Psychtoolbox Video Timing Issues/Bugs

I am hopeful this program,
with evidence collected from lots of different Mac hardware & software configurations,
will help the PTB community diagnose, debug, and fix the lingering timing issues that many of us are struggling with.

### Bug #1. VBL Sync Failure at startup

On some Mac computers, Psychtoolbox fails the VBL Sync Test on startup,
either intermittently or every time.
In contrast, in my experience, MeasureFlipInterval(1) succeeds every time, with no missed Flips.

Therefore, the Psychtoolbox VBL Sync Test Failure should be fixable by following the example code here.

### Bug #2. Extra 3 millisecond delay following vertical blanking

On some Mac computers,
there is a 3 millisecond delay between the vertical blanking event (VBLTimestamp)
and the moment just before Screen('Flip',...) finishes (FlipTimeStamp).
Based on the beam position returned,
this extra delay seems to occur before the final beam position query.

Since 3 milliseconds is a large fraction of the Flip Interval (16.67 msec), this suggests there is a bug in the code.

The function that does the real work behind Screen('Flip') is PsychFlipWindowBuffers().
This complex function is more than 1000 lines of code.  (Lines 2867 to 4048).

    https://github.com/kleinerm/Psychtoolbox-3

    PsychSourceGL / Source / Common / Screen / PsychWindowSupport.c
    
The source of the extra delay is probably related to calling a Wait function in a loop roughly 8-10 times.

    PsychWaitIntervalSeconds(0.00025);

Although this delay may have been necessary in the past for some hardware,
perhaps with some careful review this could be reduced or eliminated.
