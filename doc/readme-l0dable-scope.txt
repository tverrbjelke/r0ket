@file l0dable scope.c

The l0dable "scope" aims to be a small micro oszyilloscope.
bare bone "bus pirate"
At the "hackerbus" pins you can measure the voltage at several sampling rates.

All this is still draft work, so don't take all of this as given, 
but as roadmap milestone for release version 1.0. 


= layout =

= Navigational Structur Diagramm =
 * different scope modes exist:
 
   * mode: Continuous Sampling
   * mode: freeze-navigate, to explore the just sampled data,
           flip through the data windows, scale Y, save the shit.
   * mode: XY-Sync Sampling
   * mode: C-sync-up Sampling (_|-)  
   * mode: C-sync-down Sampling (-|_)
      * "adapt triggerpoint" (not for cont. sampling)
	 In this mode you can change the syncpoint, 
	 moving it via buttons on the display.
	 Triggering happens:
	  - In "up" mode the signal comes from below and crosses the treshold.
	  - In "down" mode, the signal comes from above and
	    strikes the treshold downward.
	 Then a kind of hsync happens,
	 the display is started from the left again. 

           
 * help screen will probably not be included, because of memory limitations. 
   Read this file instead.

@todo as navigational structure diagramm

Main Menu
	      -> Global Config
	      -> Continuous Sampling
	      -> C-sync-up Sampling (_|-)  
	      -> C-sync-down Sampling (-|_)
	      -> XY-Sync Sampling
	      -> Quit

	      
Global Config
	      -> Scope Settings
	      -> File Settings
	      -> Display Settings
	      -> Main Menu

Scope Settings
	      -> back (it is now Global Config)

File Settings
	      -> back (it is now Global Config)

Display Settings
	      -> back (it is now Global Config)


C-sync-up Sampling
	      -> freeze-navigate
	      -> Adapt Triggerpoint
	      
C-sync-down Sampling
	      -> freeze-navigate
	      -> Adapt Triggerpoint
	      
Continuous Sampling
	      -> freeze-navigate

XY-Sync Sampling
	      -> freeze-navigate
	      -> Adapt Triggerpoint

  
Freeze-Navigate
	      -> back
	      -> Save Data
	      -> Main Menu
	      
Save Data
	      -> Freeze-Navigate (= back)
	      -> Main Menu


* mode: "xy-sync" 
   * mode: "c-sync-up" (_|-)  
   * mode: "c-sync-down" (-|_)
   * mode: "adapt triggerpoint" (not for cont. sampling)
           In this mode you can change the syncpoint, 
           moving it via buttons on the display.
           Triggering happens:
            - In "up" mode the signal comes from below and crosses the treshold.
            - In "down" mode, the signal comes from above and
              strikes the treshold downward.
           Then a kind of hsync happens - the display is started from the left again. 

Maybe later I will add some shortliks, e.g. from 



= Config File =
Goal: must be simple and easy parsable (small codesize).

 * Comment starts with "#" at beginning of line 
   (maybe later we allow # also after some content=

 * configversion=1, 
   With new software versions, maybe we run into different 
   versions for the format of the config- and data-files. 
   Caveat: don't change this manually. This is for datafile conversion purpose,
   when you want to migrate old datafiles / configfiles to new version.

== File Settings ==
   
 * Filenames must fit into: 8+3 characters (old fat fstype), 
lowercase ascii, default = "scope.cfg" (scope is the name of the l0dable).

 * Datafiles: are named with round robin numbering IDs, 
   starting with 00, increasing, and when maxdatafiles *would* be reached,
   next file will be again 00 (overwriting the any existing old 00 file).
 
   * datafileprefix: max 6 characters of [:alnum:], default is 
   datafileprefix=scope_

   * datafilepostfix: max 3 characters of [:alnum:], 
     default is:
     datafilepostfix=dat
 
   * maxdatafiles: two digits [0-9][0-9]
     is the upper limit (supremum) of the generated IDs for datafiles. 
     Caveat: If the flash/diskspace is not sufficient
     you will probably run out of diskspace, while generating more and more 
     data files. So with low diskspace, keep this number sufficiently low.
     @todo: give some estimations and hints on filesizes...
     default is:
     maxdatafiles=04
     
   * currentdatafile: which datafile will be generated/overwritten next.
     Helpful, if you want to continue an existing series of measurements.
     With new session, 
     currentdatafile=00

     <num> consists of  two digits [0-9][0-9], numbering the datafiles.
     
     Caveat: 00 <= currentdatafile < maxdatafiles!

== Scope Settings ==

IO:

 * io_pinX: 
 * io_pinY: which pin to measure as Y axis.
   @todo hackerbus layout where is which pin?

 * samplerate: in [Hz] or [ms]: 
   every X ms we take a new sample.
   
   @todo make some good estimations what the hardware is capable of, 
   and how much can be stored in memory/display.

 * samplesize: how many samples for "one take", to be put into the data file.
   important when recording in continuous sampling.

 
Sync Options:
 * tresholdY: determines when to restart x-axis 

== Display Settings ==

 * scaleX: <num> (time) factor determining how much the display is scaling 
 * scaleY: <num> (voltage)
 * offX: <int>: negative or positive panning of the screen
 * offY:
 * preWindow: number of samples, before the triggerpoint was reached.
	      @todo maybe it is the same as offX? Name also is not yet good.

== Example Config File ==

#Each time the config file is changed, 
#we have a "major" update of version of the software, too
version=0.1

datafileprefix=scope_
datafilepostfix=dat
maxdatafiles=04

currentdatafile=00

io_pinY=0 

