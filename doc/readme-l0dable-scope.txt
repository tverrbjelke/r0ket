@file l0dable scope.c

The l0dable "scope" aims to be a small micro oszyilloscope.
At the "hackerbus" pins you can measure the voltage at several sampling rates.

All this is still draft work, so don't take all of this as given, 
but as roadmap milestone for release version 1.0. 

= layout =

 * main menu screen

 * global "config" Screen

 * scope screens: different scope modes exist:
 
   * mode: "continuous sampling"
   * mode: "xy-sync"
   * mode: "c-sync"
   * mode: "adapt triggerpoint" (not for cont. sampling)
   * mode: freeze+navigate

 * help screen will probably not be included, because of memory limitations. 
   Read this file instead.





= Config File =
Goal: must be simple and easy parsable (small codesize).

 * Filenames must fit into: 8+3 characters (old fat fstype), 
lowercase ascii, default = "scope.cfg" (scope is the name of the l0dable).

 * Comment starts with "#" at beginning of line 
   (maybe later we allow # also after some content=

 * configversion=1, 
   With new software versions, maybe we run into different 
   versions for the format of the config- and data-files. 
   Caveat: don't change this manually. This is for datafile conversion purpose,
   when you want to migrate old datafiles / configfiles to new version.
     
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
     
   * currentdatafile: which datafile will be generated/overwritten next.
     Helpful, if you want to continue an existing series of measurements.
     With new session, 
     currentdatafile=00

     <num> consists of  two digits [0-9][0-9], numbering the datafiles.
     
     Caveat: 00 <= currentdatafile < maxdatafiles!

 * Samplerate: in [Hz] or [ms]: 
   every X ms we take a new sample.
   
   @todo make some good estimations what the hardware is capable of, 
   and how much can be stored in memory/display.
   
 * scaleX: <num> (time) factor determining how much the display is scaling 
 * scaleY: <num> (voltage)
 * offX: <int>: negative or positive panning of the screen
 * offY:
 
 * io_pinX: 
 * io_pinY: which pin to measure as Y axis.
 

 #Each time the config file is changed, we have a "major" update of version.
 version=0.1
 datafilepattern=scope_00.dat
 