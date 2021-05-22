# What's New?
This document is meant for those who want to update their themes to the newest version, but aren't sure what needs to be added (both file-wise and line-wise), with commentary on what exactly the new items *are*.  Currently, the milestone client versions for this document are as follows:

 - v2.6.0 (This is the assumed "minimum" version)
	 - Released December 15th, 2018
 - [v2.6.2](#whats-new-in-v262)
	 - Released July 27th, 2019
 - [v2.8.4](#whats-new-in-v28x)
	 - Released July 31st, 2020
 - [v2.8.5](#whats-new-in-v28x)
	 - Released August 21st, 2020
 - [v2.9.0](#whats-new-in-v290)
	 - Released March 6th, 2021
 - [v2.9.1](#whats-new-in-v291)
	 - Released May 3rd, 2021

If you don't see a "What's new in X" for any of the aforementioned versions, that means there was no update to client themes since the previous release.
## What's new in v2.6.2?
### courtroom_design.ini

```ini
; **COORDINATE SYSTEM RELATIVE TO "viewport"**
; x/y coordinates 0,0 will start at top-left of the "viewport" for everything below until specified otherwise.
;; These icons are played whenever evidence is presented - It's the animation prior to actually showing what the presented evidence is!
left_evidence_icon = [x, y, width, height]
right_evidence_icon = [x, y, width, height]
; These two are casing-related inputs.
; "casing" is a tickbox that toggles whether you should receive case alerts or
; not (you can set your preferences, and its default value, in the Settings!)
; "casing_button" is an interface to help you announce a case (you have to be
; a CM first to be able to announce cases). 
casing = [x, y, width, height]
casing_button = [x, y, width, height]
```
### courtroom_fonts.ini
***There was no changes made since the previous version.***
### courtroom_sounds.ini
***There was no changes made since the previous version.***
### lobby_design.ini
***There was no changes made since the previous version.***
### File changes
***There was no changes made since the previous version.***

## What's new in v2.8.x?
### Brief Overview
- Added in-client iniswapping via a dropdown menu that refers to `iniswaps.ini`
- Added a visible Music Display - Now you don't need to scroll through IC logs to see what's playing!
- Added customizable emote button sizes (used to be hardcoded as a 40x40 button)
- Added a Sound Effects dropdown menu. No more need to tie Sound Effects to specific sprites (but you still can if you want to)!
- Added a visible effects dropdown, as well as the ability to let themes have their own visible effects that they can use. (You can find more on the character-specific side in the Characters Overview document)
- Additive text option (Add onto your last statement without refreshing the IC Viewport)
- Screenshakes
- Public and Private Evidence screens - No need to advertise to witnesses (and the other benches) what you have in unscripted cases!

### courtroom_design.ini
```ini
; **COORDINATE SYSTEM RELATIVE TO "viewport"**
; x/y coordinates 0,0 will start at top-left of the "viewport" for everything below until specified otherwise.
; ****
; The scrolling music name display
music_display = [x, y, width, height]
; WARNING: "music_name" x/y coordinates relative to "music_display"!
music_name = [x, y, width, height]
; Emote buttons - [490, 98] determines how many columns and rows of buttons are
; displayed per page. 49, 49 is the ABSOLUTE MINIMUM, and displays 1 button per
; page. Having either number lower than 49 crashes the client when you try to
; pick a character. If you want X columns and Y rows, you would change it to
; 49X, 49Y (ie. 490, 147 if you want 10 columns and 3 rows)
emote_button_size = [x, y, width, height]
; Display the accessible iniswaps on this character (which are grabbed from iniswaps.ini)
iniswap_dropdown = [x, y, width, height]
; The button to remove the current iniswap
iniswap_remove = [x, y, width, height]
; Display the accessible sfx on this character (grabbed from soundlist.ini). If none found, courtroom_soundlist.ini will be used.
sfx_dropdown = [x, y, width, height]
; The button to remove the current iniswap
sfx_remove = [x, y, width, height]
; Display the list of overlay effects accessible
effects_dropdown = [x, y, width, height]
; The size of the icons for dropdown entries
effects_icon_size = [width, height]
; Additive button. Allows for people to add onto their previous message without completely refreshing the IC viewport
additive = [x, y, width, height]
; Screenshake
screenshake = [x, y, width, height]
; Buttons for Loading and Saving Evidence to a file
evidence_load = [x, y, width, height]
evidence_save = [x, y, width, height]
; Buttons to transfer Evidence between public and private inventories
evidence_transfer = [x, y, width, height]
evidence_switch = [x, y, width, height]
; How large are the evidence icons?
evidence_button_size = [width, height]
evidence_ok = [x, y, width, height]
pair_order_dropdown = [x, y, width, height]
; Where the text will be aligned in the showname box.
; Possible options include:
;   left
;   center
;   right
;   justify (like in newspapers)
showname_align = 
```
### courtroom_fonts.ini
(Note: Replace `[reference]` with the proper text string)
```ini
showname_font = 
showname_color = [r,g,b]
showname_bold = [0 or 1]
showname_sharp = [0 or 1]

message_font = 
message_color = [r,g,b]
message_bold = [0 or 1]
message_sharp = [0 or 1]

ic_chatlog = [font size]
ic_chatlog_font = 
ic_chatlog_bold = [0 or 1]
ic_chatlog_sharp = [0 or 1]

ms_chatlog = [font size]
ms_chatlog_font = 
ms_chatlog_color = [r,g,b]
ms_chatlog_sender_color = [r,g,b]
ms_chatlog_bold = [0 or 1]
ms_chatlog_sharp = [0 or 1]

server_chatlog = [font size]
server_chatlog_font = 
server_chatlog_color = [r,g,b]
server_chatlog_sender_color = [r,g,b]
server_chatlog_bold = [0 or 1]
server_chatlog_sharp = [0 or 1]

music_list_font = 
music_list_color = [r,g,b]
music_list_bold = [0 or 1]
music_list_sharp = [0 or 1]

; This affects what's seen in the "music_display" element mentioned in `courtroom_design.ini`
music_name = [font size]
music_name_font = 
music_name_color = [r,g,b]
music_name_bold = [0 or 1]
music_name_sharp = [0 or 1]

area_list = [font size]
area_list_font = 
area_list_color = [r,g,b]
area_list_bold = [0 or 1]
area_list_sharp = [0 or 1]

evidence_name = [font size]
evidence_name_font = 
evidence_name_color = [r,g,b]
evidence_name_bold = [0 or 1]
evidence_name_sharp = [0 or 1]

evidence_image_name = [font size]
evidence_image_name_font = 
evidence_image_name_color = [r,g,b]
evidence_image_name_bold = [0 or 1]
evidence_image_name_sharp = 

evidence_description = [font size]
evidence_description_font = 
evidence_description_color = [r,g,b]
evidence_description_bold = [0 or 1]
evidence_description_sharp = [0 or 1]
```
***`label_color` has been depreciated, and can be set through the use of CSS Stylesheets***
### courtroom_sounds.ini
It is no longer necessary to specify the extension of the audio files
### lobby_design.ini
```ini
settings = [x, y, width, height]
server_search =  [x, y, width, height]
```
### File changes
Download for the new files in Default Theme here
- It should be noted that in this client version, most filenames are the same as their `courtroom_design.ini` and `lobby_design.ini` counterparts.
#### Added
|Images| |
|--| --|
|callmod.png                    |changecharacter.png            |
|chatblank.png                  |chat_arrow.gif                 |
|evidence_background_private.png|evidence_button.png            |
|evidence_global.png            |evidence_ok.png                |
|evidence_overlay.png           |evidence_overlay_private.png   |
|evidence_save.png              |evidence_load.png              |
|evidence_transfer.png          |evidence_transfer_private.png  |
|evidence_x.png                 |music_display.png              |
|screenshake.png                |screenshake_pressed.png        |
|reloadtheme.png                |

|INI Files|CSS FIles |
|--|--|
|character_soundlist.ini  |courtroom_stylesheets.css|

|WAV Files|(These are the interjection background audio)!!)|
|--|--|
|counteralt.wav           |gotit.wav                |
|crosswords.wav           | (Why this is here, idk)|
#### Deleted
***No files have been deleted***
#### Renamed
| 2.6.2 | 2.8.x |
|--|--|
| objection.gif | objection_bubble.gif |
|holdit.gif|holdit_bubble.gif|
|takethat.gif|takethat_bubble.gif|
|evidencebackground.png|evidence_background.png|
|deleteevidence.png|evidence_delete.png|

## What's new in v2.9.0?
### Brief Overview
- Addition of Timers
- Addition of Y-Offsets
- `iniswaps.ini` was given a new incarnation! Alongside the character-specific iniswaps, you can also setup universal iniswaps by having a `iniswaps.ini` located in the root of the `base/` folder.
- Addition of the Scrollable Character List.
### courtroom_design.ini
```ini
music_list_animated = [0 or 1]
pos_remove = [x, y, width, height]
char_list = [x, y, width, height]
pair_vert_offset_spinbox = [x, y, width, height]
; timers
; universal
clock_0 = [x, y, width, height]
; def
clock_1 = [x, y, width, height]
; pro
clock_2 = [x, y, width, height]
; def2
clock_3 = [x, y, width, height]
; pro2
clock_4 = [x, y, width, height]
```
### courtroom_fonts.ini
```ini
; universal
clock_0 = [font size]
clock_0_font =  
clock_0_color = [r,g,b]
clock_0_bold = [0 or 1]
clock_0_sharp = [0 or 1]

; def
clock_1 = [font size]
clock_1_font = 
clock_1_color = [r,g,b]
clock_1_bold = [0 or 1]
clock_1_sharp = [0 or 1]

; pro
clock_2 = [font size]
clock_2_font = 
clock_2_color = [r,g,b]
clock_2_bold = [0 or 1]
clock_2_sharp = [0 or 1]

; def 2
clock_3 = [font size]
clock_3_font = 
clock_3_color = [r,g,b]
clock_3_bold = [0 or 1]
clock_3_sharp = [0 or 1]

; pro 2
clock_4 = [font size]
clock_4_font = 
clock_4_color = [r,g,b]
clock_4_bold = [0 or 1]
clock_4_sharp = [0 or 1]
```
### courtroom_sounds.ini
***There was no changes made since the previous version.***
### lobby_design.ini
***There was no changes made since the previous version.***
### File changes
#### Added
***There was no changes made since the previous version.***
#### Renamed
|2.8.x|2.9.0|
|--| --|
|crossexamination|crossexamination_bubble.gif|
|guilty|guilty_bubble.gif|
|notguilty|notguilty_bubble.gif|
|witnesstestimony|witnesstestimony_bubble.gif|
#### Deleted
***There was no changes made since the previous version.***
## What's new in v2.9.1?
### Brief Overview
 - Updated the Settings Menu (nothing you need to worry about)
 - Added Subthemes & Parent Themes systems
 - Added compatibility with Animated UI Elements
	 - If you're having issues with any of the interjection bubbles replacing their button counterparts, rename the bubbles to `[original name]_bubble`. This is caused by AO's hierarchy of file formats, where animated images are favored over still images.
 - *COMPLETE* exposure of the client's Qt Objects. Qt CSS Stylesheets are very powerful, so beware!
 - (Something about [clipping AOImage objects](https://github.com/AttorneyOnline/AO2-Client/pull/322), reminder to get clarification from in1tiate later)
### courtroom_design.ini
***There was no changes made since the previous version.***
### courtroom_fonts.ini
***There was no changes made since the previous version.***
### courtroom_sounds.ini
***There was no changes made since the previous version.***
### lobby_design.ini
***There was no changes made since the previous version.***

---
- Documentation pulled together by Crystal, with help from Crystalwarrior
- Overviews by Crystal, with references from version changelogs
- (Almost) All annotations come from the v2.9.1 versions of the aforementioned files.
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTI0NDIwMzUzMiwtMTgxNjYyMTc1OCwxND
YxNTU2NTUwLDEyMDMxMzgyNDIsMTIxMDEzOTQ1LDMyMDI3Mjcx
NywxNjIyODQ5MDkzLC0xODUyNjE3MTk4LC0yMjgxMTY3MDAsLT
QxNjQyMTcxNCwxMDkxMjAzMzUyLC0xODIxMjIyNTY1LC01NjMy
OTEwMzAsMTk0MjQ3ODY5NywtNDIxOTYwOTcxLDU4NzIyNDUyNi
wtNzI5Njg0NzMyXX0=
-->