## Deprecated/Renamed options

Older char.inis may contain sections or options not mentioned in this guide. Here are a few of them and a brief explanation of each, for posterity:

- - `firstmode`: Technically never deprecated - `firstmode` was never used by AO2 in the first place. This is another holdover from AO1, and if you see this in a char.ini you can safely assume it's _very_ old. The purpose of `firstmode` was to dictate how many emotes were selectable on the first page of emotes, due to limitations in how Attorney Online 1.X displayed them resulting in a maximum of 20 emotes without clipping into the IC Log. It was altered in the unofficial Attorney Online 1.8 version as seen [here](https://web.archive.org/web/20201025015210/https://sites.google.com/site/attorneyonlinedev/updates/asmallpatchandastoryaboutbuttons) to allow for theoretically infinite emotes without this clipping issue, though it still affected how many emote buttons would display on the first page.

- `shouts`: (Depreciated in 2.9.0, functionality merged with `chat`) Modifies the interjections, zooms, and realization flash your character will use if they aren't included in your character folder. These are located in `misc/`, just like `chat` - in fact, they often share a folder.

- `gender`: (Renamed to `blips` in 2.9.0)

- `chat_font` & `chat_size`: (Deprecated, use chatboxes for this instead)

> **TODO:** It's possible to use static and animated emotes at the same time by including only the `(a)` or the `(b)` animation and putting an identically named static emote in the root folder. Determine if this is intended, and if it is, add a section about it here. - in1tiate


## `(c)` emotes, DemoThings, and cluttered up miscs

> **TODO:** I wrote this section and then realized it doesn't really fit in a character creation guide. As I'm certain many will find the storied history of AO1 as entertaining as I do, I suggest a dedicated page for documenting it. - in1tiate

Extremely old file bases may have characters that have files with the prefix "`(c)`". These are a remnant of a never-realized feature in AO1 that would allow the specification of a third animation to play once the character was done talking. It was never implemented, and thus `(c)` emotes went unused until 2.9.x.

Another commonality in AO1-era file bases is the presence of a "DemoThings" folder located under `misc/`. This folder's name is somewhat nonsensical - on AO1, its purpose was to store the `char_icon` of every character so that they could be displayed on the character selection screen. AO2 uses the much more sane solution of storing the icon inside the character folder.

Many assets used in the UI were also stored in `misc/` on AO1 - as themes were nonexistent - some of which have seen continued use to this day - including the ever-popular Missingno placeholder.
