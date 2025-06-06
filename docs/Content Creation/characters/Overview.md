# Creating Characters

You're here because you want to make a character! There are three simple steps to creating a character:

1. Collect content (images, animations and sounds).
2. Create a `char.ini` file that describes them.
3. Package and distribute.

## 1. Collecting Content

First, you need to find or create content that you wish to make emotes out of.

### Sizing

The ***absolute minimum*** recommended size of your content is any image with a height of 192 pixels, though most community servers will recommend minimum precise dimensions of 512x384 or 960x540. 

Keep in mind that if your image is taller than the **viewport** (the area where characters appear; its size is controlled by your theme - default being 256x192), your content will be scaled down to fit within the vertical boundaries.

That being said, here are some basic rules when the viewport is not the same size as your content:

- Content that is not exactly the same size as the viewport will be scaled according to its height.
- Content maintains its aspect ratio; if the content is wider than the viewport it will be cropped to fit, while if it is slimmer it will be centered.

### Animations

Emotes may be static or animated. For animated emotes, you'll want two animations: an _idle_ animation and a _talking_ animation. Some emotes may also have a third animation known as a _preanimation_.

You will need to name your emotes in a specific convention to be detected by the engine. For an animated emote called `foo`, the idle animation must be named `(a)foo` and the talking animation `(b)foo`. For a static emote, `foo` is enough. The preanimation can be any name (since they are often recycled).

There are a number of animated filetypes that are compatible with the engine:
- The GIF format is easy to produce due to being extremely common on the web, but has a number of limitations making it less than ideal for most situations. It's highly recommended you use any other format.
- The Animated PNG (APNG) format, unlike GIF, allows for 8-bit transparency, palettes greater than 256 colors, and smaller file sizes making it *ideal for 2D characters.* The [APNG Assembler](https://sourceforge.net/projects/apngasm/) is recommended for producing content in this format. Please note that APNG images _must_ have the extension ".apng" in order to be loaded as animations.
- Google's WebP is a format that provides not only the benefits of APNG, but also allows video-like animations to also enjoy small file sizes. The [official WebP utilities](https://developers.google.com/speed/webp/download) are recommended for producing content in this format. *Lossy WebP is recommended for 3D characters with high frame rates.*

**Beware of having multiple formats with the same file name** *(i.e. - Default.WEBP and Default.GIF)***, as the engine will prioritize filetypes in order of this list, going from highest to lowest:**

1) WebP
2) APNG
3) GIF
4) PNG

### Buttons

Once you have created your emotes, you will need to make button icons for them in an `emotions` folder inside the character folder. For each emote, there must be at least a button named `buttonX_off.png`, where X is the emote number which you will specify in the `char.ini` file. Optionally, you may also include a `buttonX_on.png` which displays when the emote is selected. If you choose not to create one yourself, it will be generated automatically when you first select that emote in the engine. 

Button icons should be an aspect ratio of 1:1 with a minimum size of 40x40 for maximum compatibility.

The character icon is always named `char_icon.png` and is a 1:1 aspect ratio with a minimum size of 60x60.

### Interjections (Shouts)

You may also wish to customize the **interjections**, each of which have a fixed name:

- "Hold it!" (`holdit.gif`|`holdit.wav`)
- "Take that!" (`takethat.gif`|`takethat.wav`)
- "Objection!" (`objection.gif`|`objection.wav`)
- Custom (`custom.gif`|`custom.wav`)

You can have more than 1 custom interjection, which the player can use by right-clicking the "Custom" button. To do this, you need to place the interjection audio and animation in `custom_objections`. The folder structure will look like this:

```
characters/
    MyCharacter1/
	    custom.gif
	    custom.wav
	custom_objections/
		awesomecustomobj.gif
		awesomecustomobj.wav
		sadcustom.apng
		sadcustom.ogg
```

Since WAV files are very large, the Ogg Vorbis (`.ogg`) or Ogg Opus (`.opus`) format may also be used for any sound effect or interjection. *Opus is recommended.*

## 2. Creating a `char.ini`

### Sample INI file

```ini
[Options]
name = Phoenix
showname = Wright
needs_showname = true
side = def
blips = male
chat = aa
effects = default/effects
realization = realization
scaling = smooth

[Shouts]
holdit_message = This is a custom Hold it! message!
custom_name = My custom shout
custom_message = This is my custom shout!
custom2_name = My second custom shout
custom2_message = This is my second custom shout!

[Emotions]
number = 13
1 = pointing#-#pointing#0#
2 = thinking#-#thinking#0#
3 = normal#-#normal#0#
4 = confident#-#confident#0#
5 = paper#-#document#0#
6 = headshake#nope#-#1#
7 = slam#deskslam#handsondesk#1#
8 = nod#nodding#normal#1#
9 = damage#ohshit#sweating#1#
10 = zoom#-#zoom#5#
11 = bashful#-#sheepish#1#
12 = coffee#phoenix-chugs#phoenix-coffee#1#
13 = despair#sweating#phoenix-emo#1#

[SoundN]
7 = sfx-deskslam
9 = sfx-stab2
13 = sfx-deskslam
14 = sfx-deskslam

[SoundT]
7 = 4
```

### `[Options]`

#### ❗❗ Options marked "(optional)" are optional! Consider leaving out options which don't need changing. ❗❗

- `name`: specifies which folder to look for character assets, i.e. this should be named the same as the character folder. (Mischievious players can change this name to something else to use another character; this is called _ini-swapping._ However, this ini-swap method is unnecessary, as AO 2.9.x introduced the ini-swap dropdown bar)

- `showname` (optional): this name will appear on the nameplate whenever the character speaks, however it is fetched locally which means if you write `showname = FINIX RAIT`, only you will see it. If you want to change your nameplate for everyone, type it in the "Showname" field while playing.

- `needs_showname` (optional, defaults to true): if false, use a blank showname.

- `side` (defaults to "wit"): modifies where in the courtroom the character initially appears. This is commonly called your **pos**, or **position**. Valid options:
  	- `def` - Defense
    - `pro` - Prosecution
    - `hld` - Helper defense
    - `hlp` - Helper prosecution
    - `jud` - Judge
    - `wit` - Witness
    - `jur` - Juror (since 2.6)
    - `sea` - Seance (since 2.6)

- `blips`: (optional, defaults to "male") modifies the sound that plays while your message text scrolls (colloquially called a **"blip"**).
  Blip sound effects can be located in `sounds/blips/` (recommended) or `sounds/general/` with the prefix `sfx-[blip]` (not recommended).

- `chat`: (optional, defaults to your theme's  chatbox) allows characters to use custom **chatboxes** (the box your message appears in) and **interjections**. Input should be a directory in `misc/` containing the chatbox you want to use.
  - For example, a character with `chat = dgs` will attempt to use the chatbox contained in `misc/dgs/`.
  - Assuming `chat_font` and `chat_size` are not set, this will also use the custom chatbox's font settings if it has them.
  > **TODO:** `misc/` folders have become somewhat more like miniature themes as of 2.8.4, and should have either a section in this guide or their own page. - in1tiate

- `effects` (optional, defaults to `default/effects`): specifies misc folder to search for overlay effects, like `chat` and `shouts`.

- `realization` (optional): specifies custom realization sound to be played; must be located in `base/sound/general`.

- `scaling` (optional): The resize mode used. For more information, see [Viewport](../viewport.md)

#### `[Options2]` etc (optional)
You may optionally define up to 4 extra `[Options]` with limited settings available - as of 2.10.2, only `showname` and `blips` may be overridden in this manner:
```
[Options2]
showname = Trilo
blips = female
```
This will only apply to the emotes which have been set to use it via `[OptionsN]` - see below.

#### `[Shouts]` (optional)
With 2.9.0, interjections are now logged in the IC logs. This sections allows for content creators to define both custom interjections, and custom messages for each character's interjection. For examples in-action, please look at Apollo's `GOTCHA!`  and Miles' `EUREKA!`

#### ❗❗ You **do not need** to define `holdit`, `objection`, or `takethat` - these values are already handled by the client itself. You should only override them when a character's default shouts are different from the default ones.
 
### `[Time]` (optional)

Made mostly redundant with release 2.8.4. The purpose of this section was to dictate the duration of pre-animations. This was a carry-over from AO1, which required it because of limitations in its engine. Up until 2.8.4, pre-animations would have to be "declared" in this section before they could be used in order to maintain backwards compatibility with AO1. `[Time]` can still be used to dictate pre-animation's duration, but it is no longer strictly necessary.


### `[Emotions]`

Onward to the `[Emotions]` section. This is where you configure what emotes your character has and how they work. The bulk of text in your char.ini will likely be here.

The `number` option is pretty self-explanatory. It specifies the number of emotes. Make sure this is correct - if it's too low, you won't be able to use all your emotes. If it's too high (or if you don't specify a `number`), you'll end up with placeholder emotes that'll clutter up your pages.

All characters will follow the format of `<emote number> = <comment>#<preanim>#<emote>#modifier[#<deskmod>]`, where `<this>` is required and `{this}` is optional.

Now for the specific emotions.

#### `<comment>`

Emote **comments** will display in the dropdown menu and on the emote button itself if an emote icon for it could not be found. You can think of this as the "name" of the emote. You should make this pretty short - if it's too long, it'll probably get shortened by your theme.

#### `<preanim>`

The next section defines the **preanimation**, or the animation played before the character actually starts speaking. If there is none, a placeholder called `-` is typically used. Preanimations can be either in the root of the character folder or stored in a subfolder (generally named `anim`). To use a preanimation file from a subfolder, you must prefix the file name with the name of the folder. For example, a preanimation of `anim/deskslam` would correspond to this structure:

```
char.ini
anim/
	deskslam.gif
```

This works with multiple subfolders as well - for example, `anim/young/damage`:

```
char.ini
anim/
	young/
		damage.gif
```

Consider using subfolders if your character has a lot of files.

#### `<emote>` (idle and speaking animations)

Between the next #'s is the name of the actual animation when the character is idle and speaking, starting with `(a)` and `(b)` in the character folder, respectively. For instance, if your animation is named "cough", then the engine expects your idle animation to be named `(a)cough.gif` on the file system, and your talking animation to be named `(b)cough.gif`.

If something like `(c)cough.gif` is provided, that (c) animation will play during the transition from speaking to idle.

The engine will also scan for other supported file formats, such as `.apng` and `.webp`, as described above.

If no animations are found for an emote, the engine will fall back to static emotes. Static emotes don't use `(a)`/`(b)` prefixes - instead, an static emote named `cough` would be expected to be in the file `cough.png`.

If you're the type who likes to organize, you can also place files in subfolders, though the method is somewhat different from that of preanimations. To use subfolders, you must prefix your emotes with a forward slash (`/`). For `(a)`/`(b)` type emotes in subfolder mode, create two folders named `(a)` and `(b)` and place your prefix-less files in the appropriate folders. For example, the emote `/angry` will search for a file named `angry.gif` in `(a)/` and `(b)/` for idle and talking respectively. The same works for (c) animations.

As another example, the emote `/def/thinking` will search for a set of files arranged like this:

```
char.ini
(a)/
	def/
		thinking.gif
(b)/
	def/
		thinking.gif
```

This is a popular way to declutter the root of your character folder. If you're creating a character for WebAO, please note that the `(a)`/`(b)` folder arrangement is not currently supported.

#### `<modifier>`

The modifier value controls pre-animations, sounds, and zooms. The valid inputs here are: 0, 1, 5, and 6.

- `0`: Tells the client to not play the pre-animation or sound effect associated with the emote, Has no effect on `[FrameSFX]` (more on that later)
- `1`: Plays the pre-animation and associated sound.
- `5`: Zoom, in which the foreground desk or witness stand will not be displayed. Additionally, the background is replaced by speed lines. Emotes with `5` will never play pre-animations.
- `6`: Same as 5, except it will _always_ play pre-animations.

#### `<deskmod>` (optional)

This option allows an emote to either force the desk/witness stand/overlay to be displayed, or force it to disappear. This takes precedence over all other factors affecting desk visibility.

- `0`: Forcibly hide the desks while this emote is displayed.
- `1`: Forcibly show the desks while this emote is displayed.
- `2`: Hides the overlay during  pre-animation, shows it again once the  pre-animation is finished
- `3`: Shows the overlay _only_ during the  pre-animation, and hides the overlay when the pre-animation ends
- `4`: Same as `2`, except the pre-animation will ignore the current character's X/Y Offsets and any the paired characters will be hidden for its duration.
- `5`: Same as `3`, except the pre-animation will ignore the current character's X/Y Offsets and any the paired characters will be hidden for its duration.

### `[SoundN]`

Under `[SoundN]`, we see a list of numbers equaling something else. The leftmost number is the emotion number and the value to the right is the sound effect associated with that emotion. In many cases, there isn't one, and therefore the line can be completely omitted. You'll notice in the sample char.ini below, only four emotes are included under `[SoundN]`. Many ini-editors will include emotes with no sounds as `= 1` - this is a holdover from AO1 and is not necessary.

Sounds specified will be searched for under `sounds/general/`. Subfolders can be used in much the same way as preanimations - see the relevant section above.

Note that the sound effect will only play when the "Pre" tick is checked, even if the emote has no valid preanimation.

### `[SoundT]`

`[SoundT]` is essentially the delay before the sound effect is played. The input it takes is in **ticks**, which are 60 milliseconds each. The minimum value is `0`, which also happens to be the default. If there is no sound, or if the sound should be played instantly, it's safe to omit the line entirely.

The only value here is `7 = 4`, which is the `deskslam` emote. As you probably know, the `deskslam` sound is only supposed to play when hands actually come in contact with the desk and not in the start of the animation; the '4' value makes sure of that. Again, each tick is 60 milliseconds, so a value of `4` causes a wait of 240 milliseconds before the sound is played.

### `[SoundL]`

This section defines which emotes (by emote number) should loop sound effects, or which sound effects in general (by name) should loop.

For each entry, the sound effect will loop if the value is `1`.

```
[SoundL]
1 = 0
2 = 1
sound = 1
```

#### `[OptionsN] (optional)`

The counterpart to the above `[Options2]` and et cetera. This section defines which emotes should use an alternate `[Options]` block, and which one to use. For each entry, the integer used should correspond to the `[Options]` block to use. In this case, `1` implicitly means the default block:

```
[OptionsN]
1 = 1
2 = 1
3 = 2
```

#### `[<emote>_FrameSFX]`

This section lists the sound effects that should play at certain frame numbers, for a specified emote.

```ini
# Replace pre-minigun with the reference to the animation for which to set this frame sfx to
[pre-minigun_FrameSFX]
# Frame number for which the specified sound should play.
10 = soj-sarge-hatch
18 = soj-sarge-extend
40 = soj-sarge-cock
50 = soj-sarge-shoot
180 = soj-sarge-retract
204 = soj-armie-drone-set
212 = soj-sarge-hatch
```

#### `[<emote>_FrameRealization]`

This section defines at which frame a screen flash should occur. (The realization sound effect is not played.)

```ini
# Replace pre-salute with the reference to the animation for which to set this screen flash to
[pre-salute_FrameRealization]
# Frame 32 screen flash = true. Does not play realization sound effect.
32 = 1
```

#### `[<emote>_FrameScreenshake]`

This section defines at which frame a screen shake should occur.

```ini
# Replace pre-salute with the reference to the animation for which to set this screenshake to
[pre-salute_FrameScreenshake]
# Frame 32 screenshake = true.
32 = 1
```

## 3. Distributing

Generally, characters are distributed in a bundle. A typical zip file looks like this:

```

base/
    characters/
        MyCharacter1/
	        README.txt
        ...
        MyCharacter2/
	        README.txt
    ...
    sounds/
    	general/
    ...

```

This reduces the cognitive load on the player, as all they have to do is drag and drop the contents of the zip file into their AO folder.

I believe that pretty much sums it up. Happy ini-editing!

---

_Much of this content was adapted from the_ [Attorney Online User Manual](https://docs.google.com/document/d/1Si-d8lsJZla-BB0lhjDAwrUmawrRaMIf1EGaVNFEE_s/edit#) _and_ [A comprehensive guide to ini-editing](https://docs.google.com/document/d/1q21JTx5ca28VsBFgE12MAEKHxTO6zyYfpYYd-nJfuVk/edit#heading=h.cpfyd4n0hpqp) _written by OmniTroid._
