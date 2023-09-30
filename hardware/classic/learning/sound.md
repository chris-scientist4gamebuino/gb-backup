
# Sound on Gamebuino Classic

[Back to Home](./../../../README.MD) | [Back to Learning for Classic](./README.MD) | [Go to Classic Reference Summary](./../reference/README.MD)

## Intro

The power of the sound library of the Gamebuino is that it allows you to play sounds and musics in the background while you play your game without having to care about it. Just ask "play this sound", and forget about it, the library takes care of everything. But if you want to create sound effects or compose chiptunes, you should understand what happens under the hood. The goal of this article is to explain you how sound is generated and structured.

## Layers

The sound is sliced in different layers in the Gamebuino sound library (just like in any tracker). We'll start from the deeper layer (wave form generation) and go up to "tracks".

### Wave forms

PWM is used at 32 KHz to generate an analog output. To be able to play several sounds at the same time, the library simply generates one waveform per channel and add them. This is done using interruptions at a rate of 56 KHz. The waveform of each channel is changed once per frame, which is at 20 Hz by default. A waveform has the following characteristics :

- Frequency
- Wave form
    - Square wave, 50% duty cycle
    - Noise

Note : You can't directly access this layer of the sound generation. The value this characteristics take will depend on the above layers : instruments and notes.

### Instruments

Instruments allow to play different kinds of sounds, not just plain square waves. It allows you to add variations in the sound of each note. An instrument is a succession of "steps", and each "step" has the following characteristics:

- step duration in number of frames (from 0 to 63)
- step waveform, it defines if this step should be played either using square wave or noise.
- step volume (from 0 to 7), it allows to create a volume envelope.
- pitch offset (from 0 to 63), to allow you to add arpeggio, glissando for example. We'll see later that you can also use commands to add effects to certain notes.

Each instrument has a "last step looping" attribute. When a note duration is longer than the instrument it's played with, the instrument will loop on the last X steps. Or won't loop if it's set to 0. Note: You can't directly access to this layer. You can't play an "instrument", you have to play a "note" using an "instrument".

### Instrument sets

An instrument set is just an array of instruments you can attach to a channel using `changeInstrumentSet()`. The ID of the first instrument in the set is 0, the ID of the second instrument is 1, and so on. We'll see later how to select which instrument ID to use when you want to play a note.

By default, instrument 0 is plain square wave and instrument 1 is noise.

### Notes

The "note" layer is the first you can directly access, using the function `playNote(pitch, duration, channel)`. As you may guess, each note has the following parameters:

- Pitch (from 0 to 58) is the ID of a predefined frequency from A#2 to D#8. If the pitch ID is equal to 63, the note is muted.
- Duration (from 0 to 255) in number of frames, each frame lasts 1/20s by default. A duration of 8 at 20 frames/second corresponds to a quarter note at 150 BPM (the most common BPM in chiptunes according to [this interesting video about chiptunes](http://www.giantbomb.com/profile/x19/blog/chip-music-and-my-attempt-to-write-in-that-style/82269/)).
- Channel (from 0 to 3) on which channel you want the note to be played. 0 for the first channel, 1 for the second channel, etc. By default there is only 1 channel so you should use the value 0. To increase the number of channels to up to 4, edit `NUM_CHANNELS` in arduino/libraries/gamebuino/settings.c.

### Commands

Commands are used to alter the way the notes are played, arpeggio tremolo and stuff like that. A command remains active on all following notes as long as it's not passed again with a null value. You can manually pass a command using `gb.sound.command(ID, X, Y, channel)`.

The command's ID defines which command to pass. Each command has an one or two parameters, X and Y. Unless specified X is from 0 to 31 and Y is from -16 to 15.

- 0: Set note volume, X: volume (from 0 to 9)
- 1: Select Instrument, X: instrument ID
- 2: Volume slide, X: step duration, Y: step depth
- 3: Pitch slide, X: step duration, Y: step depth
- 4: Tremolo, X: step duration, Y: step depth

For example, `gb.sound.command(3,4,-5)` will make a descending arpeggio by of 5 semitone every 4 frame.

### Patterns

Now that we know how to make instruments, play notes using these instruments, and alter these notes with commands, that'd be cool to be able to put all that together to make some actual music. That's why we'll use "patterns". A pattern is a mixed array containing notes and commands. It starts from the first note, and play the next one when the first reached its end. And of course it will execute commands if there are some.

**Note** : Pattern is the recommended layer level to create game sounds.

### Patterns sets

A "pattern set" is simply an array of patterns. This way you can refer to a pattern using its ID. The first pattern's ID is 0, the second is 1, and so on.

You can assign a "pattern set" to a channel using `changePatternSet(pattern, channel)`. It allows us to easily re-use pattern, which is pretty cool as it spares memory (and also because musicians a lazy).

### Tracks

To be able to easily re-use patterns from a pattern set, we use "tracks". A track is an array of pattern IDs along with a pitch offset (or transposition) to apply to each of them when they are played. To play a track, simply call `gb.sound.playTrack(track, channel)`.
