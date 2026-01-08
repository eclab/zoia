# Hammond

Hammond is a 9-drawbar Hammond Organ simulator with a Leslie.  It is effectively an additive synthesizer.

A Hammond Organ voice is built by adding together nine (roughly) sine waves of specific frequencies, produced using tonewheels (essentially metal wheels with teeth) which spin near to pickups.  The volume of each sine wave is controlled by a drawbar which is set to values from 0...8, where 8 is full volume.  The resulting waves are then summed (mixed).

You can simulate this by creating nine sine wave oscillators of the right frequencies, then pushing each one through its own VCA to set its volume, then adding them together.

Hammond Organs often are often accompanied by Leslie speaker cabinets.  A Leslie speaker is essentially a speaker horn attached to and spinning on a turntable.  When the horn is facing you, it's louder than when it is facing away from you, and so the spinning causes a tremelo effect.  Furthermore, due to the Doppler Effect, the horn rises in pitch as the horn spins towards you, and lowers in pitch as it spins away, which produces a vibrato effect.

Thus you can simulate a Leslie by simply taking the sound and pushing it through both a tremelo and a vibrato effect.

So that's what we're doing.

### Page 0: Keyboard

The front page is a 40-note keyboard module.  It does what you expect.

### Page 1: Oscillators

There are nine Oscillators producing sine ways, each with a Value in front of it to adjust its pitch relative to the keyboard.  On oscillator has an additional negative Value on the bottom right of the screen to lower its pitch.  An additional Value adjusts the pitch of all oscillators relative to the Keyboard.

### Page 2: Drawbars

Nine VCAs which control the volume of each of the oscillators.  You can change the timbre of the Organ by changing these volumes.  Unfortunately creating Value objects for specific drawbar settings would be difficult.  Instead, you can set the drawbar settings roughly as follows:

    DRAWBAR VALUE    dB
    0          		-Inf
    1          		-21
    2          		-18
    3          		-15
    4          		-12
    5          		-9
    6          		-6
    7          		-3
    8          		 0
    
### Page 3: Leslie-Out

The nine VCAs on Page 2 are then routed through a VCA biased by a Value (first row) which turn the sound on/off according to the keyboard gate, then into the Vibrato (second row), then a Tremelo (third row) for the Leslie effect, then a Hall Reverb (fourth row) for, you know, reverb, and finally to Out (fifth row).  You can reduce the vibrato width or tremelo depth if they are too strong: but the tremelo/vibrato rate are both set to 166.7ms, which I believe is the classic "fast" setting on the Leslie speaker.

### Bugs

Even though these are just sine waves, they reveal strange artifacts at different pitches, which tells me that there are probably bugs in the Zoia's sine wave generation.  It's not aliasing: sine waves are unlikely to alias at this pitch as they have only one partial each.

### Some Famous Drawbar Settings

The drawbar VCAs on Page 2 are organized as:

    111666..
    222777..
    333888..
    444999..
    555.....
   
   Here are some well known drawbar settings (the nine numbers are organized Drawbar 1 through 9, left to right).  Use the earlier Drawbar Value to dB table to set the VCAs.

	
    007740034 Alone in the City
    887724110 America (Gospel) (U)
    006606000 America (Gospel) (L)
    885324588 Blues
    888800000 Booker T. Jones 1
    888630000 Booker T. Jones 2
    888808008 Born to B3 (Gospel) (U)
    007725400 Born to B3 (Gospel) (L)
    888110000 Brian Auger 1
    888805000 Brian Auger 2
    878000456 Bright Comping
    800000888 Brother Jack
    843000000 Dark Comping
    888888888 Dark Solo A (U)
    662000000 Dark Solo A (L)
    828200002 Dark Solo B (U)
    606000000 Dark Solo B (L)
    888000888 Fat
    080080883 Fifth Organ (Gospel) (U)
    008806000 Fifth Organ (Gospel) (L)
    006802000 Flutes
    888666888 Full and High
    868868068 Full and Sweet
    888888888 Full Organ
    688600004 Funky Comping
    888800000 Gimme Some Loving
    808808008 Gospel 1
    888000008 Gospel 2
    868666568 Greg Allman 1
    888600000 Greg Allman 2
    886000040 Greg Allman 3
    888800088 Greg Rolie 1
    886400000 Greg Rolie 2
    888886666 Greg Rolie 4
    888420080 Groove Holmes (Gospel) (U)
    000505000 Groove Holmes (Gospel) (L)
    880000000 House Bass (Gospel) (U)
    008080000 House Bass (Gospel) (L)
    868600006 Jimmy McGriff 1
    883200125 Jimmy McGriff 2 (Gospel) (U)
    448650000 Jimmy McGriff 2 (Gospel) (L)
    888888888 Jimmy Smith 1 (U)
    007500000 Jimmy Smith 1 (L)
    888000000 Jimmy Smith 2 (U)
    838000000 Jimmy Smith 2 (L)
    888000000 Jimmy Smith 3 (U)
    808000000 Jimmy Smith 3 (L)
    888400080 Joey DeFrancesco
    884400000 Jon Lord
    880060000 Latin (Gospel) (U)
    006676000 Latin (Gospel) (L)
    800808000 Matthew Fisher
    868800004 Melvin Crispel
    803600000 Mellow
    007800453 Meditation Time (Gospel) (U)
    006700540 Meditation Time (Gospel) (L)
    886800300 Paul Shaffer 1
    888768888 Paul Shaffer 2
    888878678 Paul Shaffer 3
    850005000 Pink Floyd
    888800000 Power Chords
    888800000 Progessive (Gospel) (U)
    008884000 Progessive (Gospel) (L)
    006876400 Ray Charles
    808000008 Reggae
    888800000 Rock, R&B (U)
    848000000 Rock, R&B (L)
    800388888 Screaming (Gospel) (U)
    007033333 Screaming (Gospel) (L)
    008888800 Shirley Scott
    830000378 Simmering
    876556788 Shouting 1
    668848588 Shouting 2
    878645466 Shouting 3 (Gospel) (U)
    888800000 Shouting 3 (Gospel) (L)
    008400000 Slow Balllad
    068840003 Slowly
    888700000 Soft Backing (Gospel) (U)
    555400000 Soft Backing (Gospel) (L)
    808400008 Soft Chords
    678404231 Speaker Talking (Gospel) (U)
    006602024 Speaker Talking (Gospel) (L)
    888643200 Steppenwolf
    888876788 Steve Winwood
    876543211 Strings
    008000000 Sweet
    787746046 Testimony Service  (Gospel) (U)
    008800673 Testimony Service  (Gospel) (L)
    878656467 Theatre Organ (Gospel) (U)
    008844000 Theatre Organ (Gospel) (L)
    888800000 Tom Coster
    800000008 Whistle 1
    888000008 Whistle 2
    688600000 Whiter Shade Of Pale 1 (U)
    880070770 Whiter Shade Of Pale 1 (L)
    888808006 Whiter Shade Of Pale 2 (U)
    004440000 Whiter Shade Of Pale 2 (L)
    866800000 Wide Leslie
    