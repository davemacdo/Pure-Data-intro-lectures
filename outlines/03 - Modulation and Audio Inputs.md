---
title: 3. Repetition and Modulation
---

# {{page.title}}

New objects: `loadbang`, `float`, `mtof`, `counter`?

## Review previous patch

- control objects
	- `metro`, `random`, math (addition)
- audio objects
	- `*~`, `line~` and the `$1` in the message, `vline~`
- help menu
	- right-click and select help
	- ⌘E, select and copy useful bits into the patch

## A simple sequencer from "off-the-shelf" parts

- Review lesson 2 patch
	- remember what `metro` does in the patch
		- fires bang to two objects, one to control the envelope
		- second sequence to randomly select a frequence
- copy lesson 2 patch to a new patch for the sequencer
	- keep the envelope parts
	- remove the freq parts
- how do we build a counter for a seqencer?
	- requirements:
		- step forward at regular intervals
		- loop back to the beginning when it reaches the end of a sequence
		- do something different at each step
	- **Help > Browser > Pure Data > 2.control.examples > 05.counter**
		- `float`
			- `float` holds a number from it's right inlet, and sends out when it receives a bang to the hot inlet
			- Float objects are so common, they can be abbreviated to just `f`.
		- copy first example counter into working patch
		- use modulo (`%`) to create the loop
			- start with a small number
- use `select` to pick a different action for each step
	- note that we control the number of outlets on `select`
	- show with bangs
	- the extra outlet is for "none of the above"
	- use midi note names instead of frequency numbers (Google "MIDI note table" or just remember that 60 is middle C)
		- `mtof` will handle the conversion of midi to frequencies for `osc~`
	- send frequency values over to `osc~` left (hot) inlet

What else could we do with the sequencer we built?

- change pitch
- change volume
- change tempo
- change envelope
- change timbre
- stop completely?
- use the same counter with a different `%` and a different `select` to create loops at different rates 🤯

- a note on `counter`
	- does a lot more than our counter
	- built in to a Pd external
	- install with the _Cyclone_ external (already included in Purr Data)
		- Help > Find externals, search for Cyclone
		- select the one for your platform
		- now you can create `counter` objects

## Amplitude Modulation

- `osc~` is currently used in our patch to control the position of a speaker element
	- it outputs a range of -1 to 1, and we're shaping that output with `*~` at two stages
	- the last stage is the overall volume
	- the one before that is applying the envelope
- `osc~` can also be used to change the inputs or outputs of another oscillator
- amplitude modulation occurs when we use one oscillator to control (modulate) the amplitude of another oscillator
- **demo:** add amplitude modulation
	- temporarily disconnect envelope (`vline~`)
	- make a new `osc~` with a low number argument (2-5) and connect to the right inlet of `*~`
		- low modulation frequencies create a tremolo effect
	- we can affect the amount of the modulation by multiplying the output of the mod osc before we apply it to the carrier signal
	- note the fun ring effects of higher mod freqs
	- create sliders for mod freq and amounts
	- bring back the envelope controller
	- carrier sig => amplitude modulation => envelope => final volume => dac

## Frequency Modulation

Homework: Watch Rafael Hernandez's tutorial on FM Synthesis: <https://youtu.be/DgeTHuDSgC0>
