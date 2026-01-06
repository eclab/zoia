# Zoia Patches and Stuff

This is a collection of patches and documentation for the [Empress Effects Zoia](https://empresseffects.com/products/zoia), a programmable effects pedal.

## Documentation

### Bugs (and Omissions and Misfeatures)

Various flaws I've found while using the Zoia.  Empress is unlikely to fix any of these, as they've told me that, as the Zoia is largely basically out of firmware space is now 7 years old, it's not high priority.  And they're not going to open their binary format.  I find this regrettable: I still strongly support major open source software that predates the existence of their entire company.  But they've got to put food on the plate I guess.

## Patches
### Hammond
A 9-draw-bar Hammond Organ and Leslie simulator.  The front page is entirely a keyboard you can play.  I have not set up MIDI for it yet.

### Hall 1-2, Plate 1-2, Room 1-2, Ghost 1-2
Basic 1 in 2 out Hall, Plate, Room, and Ghostverb reverb patches.  Each of these sports **predelay**, a critical reverb feature which is bizzarely missing from the Zoia's reverb modules.  To enable predelay requires rolling our own mix parameter, so don't use the reverb module's mix parameter. **These are not adequately tested yet**

### Delay Hall 1-2, Pong Hall 1-2
Basic 1 in 2 out Delay + Hall Reverb and Ping Pong Delay + Hall Reverb patches.  Each of these sports **predelay**, a critical reverb feature which is bizzarely missing from the Zoia's reverb modules.  To enable predelay requires rolling our own mix parameter, so don't use the reverb module's mix parameter.  You can use the Delay's mix parameter to determine how much delay gets pushed into the reverb.  **These are not adequately tested yet**