---
title: 4. Audio files and inputs
---

# {{page.title}}

New objects: `readsf~`, `key`, `adc~`, `env~`, `dbtorms`, `sigmund~`, `delwrite~`, `delread~`

Resource: [Freesound](https://freesound.org)

Media: [bells-cluster.wav](../media/bells-cluster.wav) - [[source](https://freesound.org/people/InspectorJ/sounds/339822/)]

## Review previous patch

- [arpeggiator-2.pd](../other%20patches/arpeggiator-2.pd)
- Floats are weird to wrap your brain around at first, but they're very powerful.
	- remember: **hot** inlets send outputs immediately, **cold** inlets take in data and wait
- `expr` is handy for stringing arithmetic operations together

## Loading samples and playing them back

- find the sound file you want (pick something small)
	- record your own
	- download something from freesound
	- example above (bells-cluster.wav)
- put the file in the same folder as your patch
- `readsf~`
	- simplest way to play back audio (note: very limited!)
	- argument sets number of channels (default is mono!)
	- output to `dac~` (can go through `*~` to adjust volume or create an envelope with `line~` or `vline~`)
	- takes in messages for controls
		- `open [filename]` (if giving a file name alone, it must be in the same folder as the patch, otherwise a relative or absolute path can work)
		- give a second or two to load the file, then send a `1` message to start playback and a `0` to stop it
		- _IMPORTANT! The sound file is cleared from memory after it is done or stopped. It must be reloaded each time._
	- outputs a bang from the rightmost outlet (after the audio channels) when playback completes
		- **demo:** handling "done" signals
			- hook up a bang to the rightmost outlet for debugging
			- use "done" outlet to reload the soundfile so it is ready the next time you need it
- more powerful audio playback can be achieved with `soundfiler` and `tabread4~`. You can read about them in Help.

## Triggering events from the keyboard

This one is pretty simple.

- `key` watches keyboard inputs and sends a number for each "printing" key to the outlet each time the key is pressed.
	- use a number box to figure out which is which
	- use `select` to grab the numbers you want to have them do stuff like play back a sample or change notes in our arpeggiator from earlier
- great for simple triggers (like "start the next section's audio track", "start a counter", "release a sustained sound", "toggle an audio effect on or off")
- limitations
	- holding a key can create different results based on system settings (recall the result of holding down a letter key while typing in a text document)
	- operating system settings might change what the code numbers are, and people in other countries may have different keyboard layouts and numbers
	- Pd must be the frontmost application to get these messages.
- more powerful alternatives:
	- MIDI
		- go to **Help > Pure Data > 5.reference > midi-help.pd** for lots of useful examples of the midi objects in Pd
	- OSC (`oscparse` receives and `oscformat` sends)
		- more powerful, but a little trickier to use
		- [TouchOSC](https://hexler.net/software/touchosc) (iOS and Android app) is a great way to build a touch interface for your patches. Possibly the best cheap ($5) tool in computer music.

## Microphone inputs

Meet `adc~`, `dac~`'s backward cousin.

- check your audio settings first
	- **Media > Test Audio and MIDI...**
- **demo:** follow incoming audio level with `adc~` and `env~`
	- make an `adc~`, note that like `dac~`, it will use your default audio device and assume you want it to be stereo (can be altered with arguments like `dac~`)
	- incoming stream is audio data
	- convert to volume levels with `env~`
		- takes an incoming audio stream (could be a file or mic)
		- outputs the envelope level _in decibels_ (show with number box)
		- decibels are tricky to work with mathematically, so we want to convert the range to our usual 0-1 scale with `dbtorms`
		- show RMS volume with number box
	- _What is this number good for?_
		- controlling the envelope of a synth
		- controlling the pitch or timbre of a synth
			- do some math to map an appropriate range for pitch, amplitude, modulation index, etc.
	- other info from the incoming signal: `sigmund~`
		- audio inlet, number outlets
		- left outlet: pitch
			- MIDI number, with decimals for pitch variation
			- only runs if there is enough volume and pitch clarity
			- filter out unpitched things with `select -1500`
		- right outlet: identical to `env~`

## Intro to live DSP

Digital signal processing. _Have a quick trigger finger on **cmd-.** or **ctrl-.** when playing with these!_

- Use `delwrite~` and `delread~` to create a basic digital delay (there are more powerful options, but this is a good start)
- open the helpfile for `delread~` as an example
- `delwrite~` stores incoming audio to a buffer (temporary quick-access storage)
	- name of the delay buffer (could be whatever makes sense to you)
	- number of ms to store in the buffer (_This is the longest delay possible with this particular buffer. It is not the actual delay time._)
- `delread~` plays back from the buffer
	- name of delay buffer
	- number of ms to delay (_This is the actual delay we hear!_), delay time can be changed dynamically by sending a new number to the inlet!
- multiple `delwrite~`s to the same buffer causes problems, but multiple `delread~`s does not.
- **demo**: simple delay "pedal"
	- copy the `delread~` helpfile to a new patch
	- clean up the comments
	- change the delay buffer name! (important, as the helpfile is still trying to write to that same buffer, or you could just close the helpfile)
	- remove `sig~` and replace with input from `adc~`
	- **before the next step:** Find the mute button for your speakers and/or get ready to kill DSP (cmd-.)
	- remove snapshot and replace with output to `dac~`
	- _How can we control this better so we can more elegantly (and programmatically) turn the delay line off or on?_
		- scale `adc~` and/or `dac~` with `*~` to 1 or 0
		- ramp up or down with `line~`
- related objects:
	- `delread4~` is more complicated but more powerful
	- `delay~` is powerful and a little simpler than `delread4~`, but requires the cyclone extension (**Help > Find externals** and search for **cyclone**)
	- `freeverb~` works similarly (actually, it's much simpler) to process audio live
