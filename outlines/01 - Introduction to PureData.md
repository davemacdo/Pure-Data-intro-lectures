# 01. Introduction to Pure Data

## Download and install

- [puredata.info](http://puredata.info/)
- Multiple versions: 
	- plain ol' Pd (Pd Vanilla)
	- Pd-L2Ork (Purr-Data) - extra stuff added in

## What is Pd?

- graphical programming language
	- uses objects on a canvas connected by lines
	- not (much) text is involved
- shows data flow and audio signal flow paths
- _runs in real time while being edited_, unlike most other programming languages
- completely open for users to build _anything_
	- nothing is "built-in"
	- not a DAW
	- no presets
	- not an effects pedal or a synthesizer (though you can make either of those!)

## Tour of Pd

- terminal window
	- output only
- patcher window
	- where you make stuff
	- note: Edit Mode (⌘E)
- patcher elements - see the Put menu
	- Object (⌘1)
	- Message (⌘2)
	- Number (⌘3)
	- GUI elements 
		- bang (⌘⇧B)
		- toggle (⌘⇧B)
		- Vslider (⌘⇧V)
		- Hslider (⌘⇧J)
	- **demo**: Make an Object, Message, Number, and Bang
		- note that the inlets and outlets
		- type something "wrong" in the Object, note the response in the terminal
		- type "print" in the Object, note the change (solid outline, inlet)
			- type "PRINT" and note the error: case-sensitive
		- move objects around and connect Bang to Print
		- turn off Edit mode (⌘E) and click Bang
		- turn on Edit mode
		- note the temporary suspension of Edit mode by holding cmd (Mac) or ctrl (Win)
		- delete and remake connection
		- move Message and type something in the Message, connect Message to Print
		- send Message and note terminal, send Bang
		- connect Bang to Message as well, note Bang now goes to print _and_ Message box
		- right-click Print to show helpfile 
			- helpfiles are all regular Pd files, are interactive, and can be copied into your current patch and edited
		- connect Number to print, show how to change number value by clicking and dragging. Note that it sends its value each time it changes.
		- connect Bang to number, show how banging the number sends its value without changing it

## My first noisemaker

- **KEYBOARD SHORTCUT: CMD-.**: This is the "panic" button. Most good audio systems have something like this. Cmd-. is a common one in software.
- new objects: 
	- `[osc~]` and `[dac~]`
	- **demo:** Make `osc~` and `dac~` objects
		- what do you think OSC and DAC are? 
			- sine oscillator and digital-to-analog (or digital-to-audio) converter
		-  connect outlet of `osc~` to left inlet on `dac~`
		- note the thicker line, indicating an audio stream, rather than a data stream
		- osc~ has two inlets, left side is frequency (right is phase, but that's not important right now)
		- make a Number and connect it to `osc~`
		- scroll up and down to slide around in pitch
- new element: VSlider
	- create VSlider from Put menu
	- right click > Properties
	- change features of slider (range to reasonable range, lin to log)
	- set range
	- set scale
	- connect to number box
	- connect direct to `osc~` and use number only for debugging
- new object: `[*]` (multiplication)
	- note that it has two inlets
	- **demo:** existing frequency number box to `*` with an argument and then to a new Number after to see result
	- typing a number as an "argument" sets a default thing to multiply by
	- change the number by sending a new one to the right inlet
	- **demo:** Make new number and connect to right inlet. Change by dragging.
	- note that the right inlet is "cold" (doesn't send output), the left inlet is "hot" (causes output each time a new number comes in)
- new object: `[*~]` (audio multiplication)
	- **demo:** volume adjuster
		- with audio running, disconnect `osc~` and `dac~`, note silence!
		- put `*~` between osc and dac with argument of 1
		- change argument to smaller numbers (not larger!)
		- create slider input for right input
		- use properties to change range from 0 - 1. 

## Resources

- [puredata.info](http://puredata.info) - This is about as close to an official website as there is for Pd. (The real [official site](http://msp.ucsd.edu/) is not very useful for much beyond downloading the tool.)
- [Pd forums](https://forum.pdpatchrepo.info/) - There are nice people here. Be nice and ask specific questions if you are looking for help. Or, you can just search their very useful archives. 
- [Pure Data Tutorials](https://www.youtube.com/playlist?list=PL12DC9A161D8DC5DC) by Rafael Hernandez - Hernandez offers some succinct introductory videos that are a good place to start. When he refers to "Pure Data extended", note that it no longer is supported. The modern equivalent is Pd-l2Ork, or [Purr Data](https://github.com/jonwwilkes/purr-data/releases). If you're using Pd on the iMacs in the lab, they're running "vanilla" Pd. Everything he demonstrates here will work fine in both places. Also, you don't necessarily need to watch them in sequence. Once you get through the basics, feel free to jump around if you see a topic that interests you. 
