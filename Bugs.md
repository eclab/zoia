# Zoia Bugs, Omissions, and Misfeatures

Zoia has a significant number of bugs, omissions, and misfeatures.  Empress told me that they do not intend to fix them.  This is frustrating because many of these could be handled with minimal effort.  So I list them here, some with workaround, some in the hopes that we might find workarounds for them.

## Bugs

### Starred parameters are extremely slow and create encoder skips

This is a major bug, and it makes starred parameters useless.  Choosing a starred parameter and editing is in many cases profoundly slow and sometimes possible because it ignores encoder movement.  However if you pick the starred parameter, then manually click on the pad in question, you can edit it smoothly and cleanly.  This is a serious bug and appears to be due to incorrect encoder polling.

### Sine waves creates sound artifacts

Sine waves -- which shouldn't be able to have any aliasing at all -- create significant sound artifacts at various pitches.  This should never happen.  See the Hammond.bin file in this repository for example.

### Delete Patch is Inconsistent with Copy Patch and Broken

Delete Patch doesn't delete and remove a patch: it *reinitializes the patch*.  There is no way to delete a patch!

This is obviously broken, but it also makes copying patches very hard.  Apparently the Zoia originally had a Copy Patch command which copied a patch to another one by overwriting the target file.  But they changed it to *insert* a new file copy before a target file.  This was done because so many people were overwriting files by accident.  

Okay, but that means that to properly copy and overwrite a patch, you would need to delete the target patch, then copy and insert the new copy.  Which you can't do because you can't delete patches.  
	
I have been bitten by this every single time I've programmed the unit. 

**Recommended Fix** Provide a real delete operation.  Get rid of the current "Delete" function, but provide a way to clear current memory.


### Save is Broken

Save does not track which patch you are saved to.  

1. Select (Load) patch 5.
2. Move patch 5 to position 20.
3. Save.  It will try to overwrite patch 5 again.

If you do any file operation before saving, you will need to manually keep track of where you moved your patch.

Save also has a nasty little gotcha, because the Save button is overloaded and used as an "okay" button for file operations.  For example, when you move a patch, normally you select the patch you want to move, then press Move, then select where to move it to, then press the encoder, and final press Save to confirm.

Multiple times, I have accidentally pressed Save without pressing the encoder first.  You'd imagine that the Zoia would helpfully tell you that you forgot to press the encoder.  But no.  Instead, it starts **saving** the file on where you had selected.  **Oh no!** 


### Move is Broken

This is minor but irritating.  You can't move a patch to the end of the list, that is after patch 63.  The only way to do that is to (1) move it to patch 62, then (2) move patch 63 to before patch 62.  No, really.

### Patch Names Line Breaking is Broken

If you name a patch "Hall 1-2", it'll be displayed on one line in the small font rather than on two lines in the large font.

## Misfeatures

### Proprietary File Format

I am the author of a large open source patch editor library, and was hoping to make an editor for the Zoia.  However the Zoia's patch file format is proprietary.  There is a reverse engineered file format sufficient to **read** a patch, more or less, and so support a librarian and a patch viewer, it is not sufficient to **write** a patch and so support a patch editor.  Empress are not willing to open their spec, so that's all we have. I do not understand why companies do this given how detrimental it is to their ecosystem, but there you go.

### No System Exclusive (Sysex) Support

The Zoia supports both MIDI In and MIDI Out, but incredibly has no support at all for system exclusive to upload or download patches.  The only way to upload a patch is to load it onto a specific folder on a Micro SD card, then insert it into the Zoia and instruct the Zoia to load it, then wait.  Downloading is similarly cumbersome.

In order to build a patch editor, I would really require one single sysex command: sending a patch over MIDI from the computer to the working memory of the Zoia.  A very simple piece of code.  But unless Empress implements this, an editor is not going to happen.  Also I note that Empress doesn't have a MIDI Manufacturer's ID (required for sysex, like $300 a year), making this even less likely.


## Omissions

### Reverb Modules do not have predelay

This is very surprising.  The reverb modules have no predelay setting at all!  That is not a small omission.  Fortunately you can work around this by manually adding in a delay line, but then you have to bypass the reverbs' mix facility and build your own mix procedure.  Here is how you'd have to work around this if you wanted stereo in / stereo out.

    AUDIO INPUT
    AUDIO OUTPUT
    DELAY LINE 1
    DELAY LINE 2
    HALL REVERB			(mix = 100%)
    AUDIO BALANCE 1
    AUDIO BALANCE 2
    VALUE	1				value = chosen wet/dry mix
    VALUE	2				value = chosen delay length
   
    AUDIO INPUT input L -> DELAY LINE 1 Audio In
    AUDIO INPUT input R -> DELAY LINE 2 Audio In
    DELAY LINE 1 output -> AUDIO BALANCE 1 audio in1
    DELAY LINE 2 output -> AUDIO BALANCE 2 audio in1
    DELAY LINE 1 output -> HALL REVERB input L    
    DELAY LINE 2 output -> HALL REVERB input R
    HALL REVERB output L -> AUDIO BALANCE 1 audio in2
    HALL REVERB output R -> AUDIO BALANCE 2 audio in2
    AUDIO BALANCE 1 audio out -> AUDIO OUTPUT output L
    AUDIO BALANCE 2 audio out -> AUDIO OUTPUT output R
    VALUE 1 cv output -> AUDIO BALANCE 1 mix
    VALUE 1 cv output -> AUDIO BALANCE 2 mix
    VALUE 2 cv output -> DELAY LINE 1 delay time
    VALUE 2 cv output -> DELAY LINE 2 delay time
    
Yikes.

### Insufficient connection attenuation / scaling

It's rather unfortunate the Zoia doesn't allow connection attenuation to scale more than 398.1%.  This causes a lot of common tasks to require two or three Value modules to create sufficient chains of scaling (see the keyboard examples below).  **It'd have been much better if the scaling went to at least 1600%.**


### Missing CV slider module

The Zoia relies heavily on turning and pressing its one encoder knob, which has a reputation for wearing out.  There are few other options for selecting values using the pads or the stomp switches.  One surprising omission is the lack of a pad slider.  Imagine a module consisting of a row of 8 pads, let's call them pads 0 through 7.  When you click on pad X in that row the module, the pad is highlighted and the module outputs the CV value X / 7: that is, it outputs a value 0...1 inclusive depending on the pad chosen.  You could make this row shorter (4 pads) or longer (16 or even 40 pads).  Every pad can be connected as a CV output, so there's no reason to have a dedicated CV output pad.  This way you could create multiple parameter sliders on your front page.  The last CV value selected for a pad is saved with the pad when you save out the patch and used to initialize the pad when you load the patch.

Given that the Zoia is criminally missing any potentiometers or utility encoders, I am amazed that this module does not exist.

### Keyboard does not save CV state

Okay, so there's no CV module -- but there's a Keyboard module which *almost* could do the same task.  But it doesn't quite work.

In the Zoia's Keyboard module you press a pad, and a CV corresponding to its note is emitted, along with a gate and a trigger.  You can customize the CV output of each pad.  Unfortunately when a patch is first loaded, the CV value is initialized to the lowest note on the pad.  This makes it impossible to use the Keyboard to output CV for other purposes, such as a poor-man's replacement for the missing slider pad discussed earlier.  Instead, when you save out a patch, the Keyboard should be saved out with the CV of its most recently pressed pad; and that should be used as the initialization value.  It's an easy fix, and one that would provide a major improvement to the interface, but it's not going to happen.

Still, if you'd like to use the Keyboard as a CV Slider, realizing you can't save state, there are two approaches.  First you could custom-set each note value independently.  You'd press a Note, and read at KEYBOARD note out.

*KEYBOARD 4 Notes Long values:*

    0.0, 0.333333, 0.666667, 1.0

*KEYBOARD 8 Notes Long values:*

    0.0, 0.142857, 0.285714, 0.428571, 0.571429, 0.714286, 0.857143, 1.0

*KEYBOARD 16 Notes Long values (this is tedious, perhaps do Method 2):*

    0.0, 0.0666667, 0.133333, 0.2, 0.266667, 0.333333, 0.4, 0.466667, 
    0.533333, 0.6, 0.666667, 0.733333, 0.8, 0.866667, 0.933333, 1.0


Second, you could just scale the keyboard's default note CV values (which weirdly start at 0.3250, not 0) to between 0 and 1.  The exact structure to scale this would depend on the length of the keyboard.  Here is the connection graph and magic numbers for keyboards 8, 16, 24, and 40 long:


 *KEYBOARD 8 Notes Long*

    KEYBOARD    8 notes
    VALUE(1)	output = -1 to 1, number = -0.3250
    VALUE(2)	
    VALUE(3)
    KEYBOARD note out -> VALUE(2) number		398.1%
    VALUE(1) cv output -> VALUE(2) number		398.1%
    VALUE(2) cv output -> VALUE(3) number		250.8%

Press a Note.  Read at VALUE(3) cv out.  Ranges from 0.0005 to 0.9992.  You can cover over the 3 KEYBOARD outputs with other stuff.


*KEYBOARD 16 Notes Long*

    KEYBOARD    16 notes
    VALUE(1)	output = -1 to 1, number = -0.3250
    VALUE(2)	
    VALUE(3)
    KEYBOARD note out -> VALUE(2) number		398.1%
    VALUE(1) cv output -> VALUE(2) number		398.1%
    VALUE(2) cv output -> VALUE(3) number		115.8%

Press a Note.  Read at VALUE(3) cv out.  Ranges from 0.0002 to 0.9997.  You can cover over the 3 KEYBOARD outputs with other stuff.  We have to use three VALUE modules because the Zoia 


*KEYBOARD 24 Notes Long*

    KEYBOARD    24 notes
    VALUE(1)	output = -1 to 1, number = -0.3250
    VALUE(2)	
    KEYBOARD note out -> VALUE(2) number		297.1%
    VALUE(1) cv output -> VALUE(2) number		297.1%

Press a Note.  Read at VALUE(2) cv out.  Ranges from 0.0093 to 0.9997.  You can cover over the 3 KEYBOARD outputs with other stuff.


*KEYBOARD 40 Notes Long*

    KEYBOARD    40 notes
    VALUE(1)	output = -1 to 1, number = -0.3250
    VALUE(2)	
    KEYBOARD note out -> VALUE(2) number		179.0%
    VALUE(1) cv output -> VALUE(2) number		179.0%

Press a Note.  Read at VALUE(2) cv out.  Ranges from 0.0001 to 0.9998.  This will be harder to set up because it'll use two pages, and you'll have to resize the keyboard multiple times.

### No Power Switch

I mean, really.
