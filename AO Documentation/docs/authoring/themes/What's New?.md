# What's New?
This document is meant for those who want to update their themes to the newest version, but aren't sure what needs to be added (both file-wise and line-wise), with commentary on what exactly the new items *are*.  Currently, the milestone client versions for this document are as follows:

 - v2.6.0 (This is the assumed "minimum" version)
	 - Released December 15th, 2018
 - v2.6.2
	 - Released July 27th, 2019
 - v2.8.4
	 - Released July 31st, 2020
 - v2.8.5
	 - Released August 21st, 2020
 - v2.9.0
	 - Released March 6th, 2021
 - v2.9.1
	 - Released May 3rd, 2021

If you don't see a "What's new in X" for any of the aforementioned versions, that means there was no update to client themes since the previous release.
## What's new in v2.6.2?
### courtroom_config.ini

```ini
; **COORDINATE SYSTEM RELATIVE TO "viewport"**
; x/y coordinates 0,0 will start at top-left of the "viewport" for everything below until specified otherwise.
;; These
left_evidence_icon = 
right_evidence_icon = 
casing = 
casing_button = 
```
## What's new in v2.8.x?
### Brief Overview
- Added 
### courtroom_config.ini
```ini
; The scrolling music name display
music_display = 
; WARNING: music_name x/y coordinates relative to music_display!
music_name = 
; Emote buttons - [490, 98] determines how many columns and rows of buttons are
; displayed per page. 49, 49 is the ABSOLUTE MINIMUM, and displays 1 button per
; page. Having either number lower than 49 crashes the client when you try to
; pick a character. If you want X columns and Y rows, you would change it to
; 49X, 49Y (ie. 490, 147 if you want 10 columns and 3 rows)
emote_button_size = 
; Display the accessible iniswaps on this character (grabbed from iniswaps.ini)
iniswap_dropdown = 
; The button to remove the current iniswap
iniswap_remove = 
; Display the accessible sfx on this character (grabbed from soundlist.ini). If none found, courtroom_soundlist.ini will be used.
sfx_dropdown = 
; The button to remove the current iniswap
sfx_remove = 
; Display the list of overlay effects accessible
effects_dropdown = 
; The size of the icons for dropdown entries
effects_icon_size =
; Additive button. Allows for people to add onto their previous message without completely refreshing the IC viewport
additive = 
; Screenshake
screenshake = 
; **COORDINATE SYSTEM RELATIVE TO "viewport"**
; x/y coordinates 0,0 will start at top-left of the "viewport" for everything below until specified otherwise.
; ****
; Buttons for Loading and Saving Evidence to a file
evidence_load = 
evidence_save = 
; Buttons to transfer Evidence between public and private inventories
evidence_transfer = 
evidence_switch = 
; How large are the evidence icons?
evidence_button_size = 
evidence_ok = 
pair_order_dropdown = 
; Where the text will be aligned in the showname box.
; Possible options include:
;   left
;   center
;   right
;   justify (like in newspapers)
showname_align = 
```

## What's new in v2.9.0?
### Brief Overview
- Addition of Timers
- Addition of Y-Offsets
### courtroom_config.ini
```ini
music_list_animated = 1
pos_remove = 
char_list = 
pair_vert_offset_spinbox = 
; timers
; universal
clock_0 =
; def
clock_1 = 
; pro
clock_2 = 
; def2
clock_3 = 
; pro2
clock_4 = 
```
## What's new in v2.9.1?
### Brief Overview
 - Updated the Settings Menu (nothing you need to worry about)
 - Added Subthemes & Parent Themes systems
 - Added compatibility with Animated UI Elements
	 - If you're having issues with any of the interjection bubbles replacing their button counterparts, rename the bubbles to `[original name]_bubble`. This is caused by AO's hierarchy of file formats, where animated images are favored over still images
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTQyMTk2MDk3MSw1ODcyMjQ1MjYsLTQyMT
k2MDk3MSwtNzI5Njg0NzMyXX0=
-->