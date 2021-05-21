## Other Content

There are some other minor types of assets that are customizable. Like all assets, other players must already have them in order for them to be visible.

### Evidence

Evidence is located in `base/evidence`, is typically 70x70, and contains a border. (It is also theoretically possible to generate a border with Qt CSS, instead of drawing a border inside the image.)

### Sound Effects

Sound effects are located in `base/sounds/general` and can be `.wav`, `.ogg`, or `.opus`. There is no restriction to length. Typically, a sound effect will stop once another character begins talking.

#### Blips

Since 2.8, you can add blips in the `base/sounds/blips` folder instead of in `base/sounds/general`.

### Music

Music tracks are located in `base/sounds/music` and can be `.mp3`, `.ogg`, or `.opus`. Transcoding music to Opus at a bitrate of 64-96k is recommended to save space at extremely minimal quality loss.

Since 2.8, you can create seamless A-B loops in tracks. Create a config file with the same name as the song, including the extension, ending in `.txt` (e.g. `song.opus.txt`). The configuration must have `loop_start` and either `loop_length` or `loop_end`. All time values are in terms of samples (use Audacity to convert seconds to samples).

### Character Themes

Since 2.8, you can define a new character theme as a separate folder inside `base/misc`. Character themes, unlike UI themes, allow the chatbox and interjection effects to be customized at a per-character level.

### Overlay Effects

Since 2.8, you can overlay animations on top of your current character to convey extra emotional range.

Effects are searched in:

- `base/themes/<current theme>/effects`
- `base/misc/…`, where `…` is the name of an effect folder defined in `char.ini` under an `effects = …` entry.

In both cases, `effects.ini` must be present in the effect folder so that the client can determine which effects can be displayed.

Supported effect formats are the same as character animation formats (GIF, PNG, APNG, WebP).

Effect icons must be defined in an `icons` subfolder inside the effect folder and should be no bigger than 16x16.

Effects can also define sound effects in `effects.ini`. The format for `effects.ini` is as follows:

```ini
effect_name = sfx-name
```

Note that character themes and effect folders can overlap; there is nothing stopping them from doing so.
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTk5NDAwNzk3OV19
-->