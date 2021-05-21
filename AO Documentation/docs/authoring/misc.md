## `misc` folder

Formerly home to everything that didn't belong in the other folders, with the introduction of themes, the `misc` folder has become vacant. However, it was given a new purpose as the go-to location for some of AO's extra customization potential.

### Character Themes

Starting with 2.8, you can define a new character theme as a separate folder inside `base/misc` through `chat = ` in the `char.ini`, allowing characters to use custom **chatboxes** (the box your message appears in) and **interjections**. Input should be a directory in `misc/` containing the chatbox you want to use. (i.e. - `chat = Phoenix Wright` would point to `base/misc/Phoenix Wright`)

### Overlay Effects

Starting in 2.8, you can overlay animations on top of your current character to convey extra emotional range.

Effects are searched in:

- `base/themes/<current theme>/effects`
- `base/misc/…`, where `…` is the name of an effect folder defined in `char.ini` under an `effects = …` entry.

In both cases, `effects.ini` must be present in the effect folder so that the client can determine which effects can be displayed.

Supported effect formats are the same as the character formats (GIF, PNG, APNG, WebP).

Effect icons must be `base/misc/[effects folder]/icons` inside the effect folder and should be no bigger than 16x16.

Effects can also define sound effects in `effects.ini`. The format for `effects.ini` is as follows:

```ini
effect_name = sfx-name
effect_stretch = false
effect_under_chatbox=true
```
Each entry is self-explanatory, but to explain what each one does:

 - `effect_name`: The name given to the effect, which will be shown in the Effects Dropdown menu.
 - `effect_stretch`: (Introduced in 2.9.0) Affects whether or not the effect stretch/squish with the viewport or not, if the aspect ratio differs from that of the viewport.
 - `effect_under_chatbox`: (Introduced in 2.9.0) 

Note that character themes and effect folders can overlap; there is nothing stopping them from doing so.

***Currently, there is a known compatibility bug where effects made during 2.8.x do not appear in the Effects Dropdown Menu for 2.9.x clients.***
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTgzNzQ0NTE0M119
-->