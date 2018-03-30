---
title: 2. Envelopes and Metro
---

# {{page.title}}

New objects: line~, vline~, metro, random

## Review previous patch

- Patcher Elements
	- connections
	- inlets
	- outlets
- Sliders
	- right-click to show properties
- Numbers
	- drag to change in read/perform mode
- Objects
	- inlets and outlets differ depending on object type
	- objects that deal with audio signals get ~ at the end of the name

## Smoothing things out with line~

- Note the noise artifacts from moving the amplitude slider
	- _Why do those occur?_
	- Answer, the clicks are the result of the computer trying to change the waveform instantly, which creates a vertical jump in an otherwise smooth sine curve. 
- Solve artifacts with `line~`
	- creates a smooth change over time
	- takes a Message as input
		- message formatted with two numbers: the target value, and the amount of time to reach it (in ms)
	- **demo:** line~ to control volume
		- disconnect volume slider
		- connect new line~ to right inlet of multiplication
		- create a message box that goes to 0.8 over 400ms
		- create a second message box that goes to 0 over 1000ms
	- _What's the problem with this approach to smoothing out our slider behavior?_
		- we can't make enough Messages for the levels of volume we would want.
	- Message variables take input and incorporate them into the output. They're number with $ in front
	- **demo:** line~ with message variables to smooth out slider numbers
		- replace 0.6 (or whatever value) with $1
		- discuss second number (something short is good, 50ms or smaller)

## Generating an envelope with vline~

- Note that using this slider for setting volume is nice, but it's not very practical for attacks
- vline~ allows us do the equivalent of stringing several line~ objects together
	- takes a message object like line~
	- uses a comma to separate line segments: 
		- first segment is _target_ and _time_
		- second and later are _target_, then _time_, then _offest_
		- the offset is from the beginning of the vline
	- **demo:** create a triggerable envelope
		- Add a new *~ element above the final volume slider
		- connect new vline~ to right inlet of new *~
		- connect new Message to vline~ that defines an attact and a release

## Automating with metro

- This envelope is cool, but it would be cooler if we didn't have to click it each time we wanted something to happen!
- Metro is short for metronome and it behaves pretty close to what you might expect
- **demo:** automate envelope triggering with metro
	- create a metro and connect to vline~'s message
	- give metro a creation argument in ms
	- create a toggle
	- start em up
	- add a number box (or slider) to the right inlet of metro
		- _What's happening here?_
		- You're overriding metro's default argument with a new input.
	- 
