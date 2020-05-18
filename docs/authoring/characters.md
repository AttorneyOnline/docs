## Creating Characters

You're here because you want to make a character! There are three simple steps to creating a character:

1. Collect content (images and animations). The recommended size is 256x192.
2. Create a `char.ini` file that describes them.
3. Package and distribute.

This section will mainly focus on INI editing.

### 1. Collecting Content

Find or create content that you wish to make emotes out of. The recommended final size for your emotes are 256x192.

Emotes may be static or animated. For animated emotes, you'll want to make two animations: an *idle* animation and a *talking* animation. Some emotes may also have a *preanimation*.

You will need to name your emotes in a specific convention to be detected by the engine. For an animated emote called `foo`, the idle animation must be named `(a)foo.gif` and the talking animation `(b)foo.gif`. For a static emote, `foo.png` is enough. The preanimation can be any name (since they are often recycled).

You will encounter certain technical limitations of GIF rather quickly. There are two other animation formats other than GIF, though support is a bit shaky for them:

- The animated PNG (APNG) format allows 8-bit transparency, palettes greater than 256 colors, and smaller file sizes. The [APNG Assembler](https://sourceforge.net/projects/apngasm/) is recommended for this.
- Google's WebP is a format that provides not only the benefits of APNG, but also allows video-like animations to also enjoy small file sizes. The [official WebP utilities](https://developers.google.com/speed/webp/download) are recommended.

Once you have created your emotes, you will need to make button icons for them in an `emotions` folder inside the character folder. For each emote, there must be an "on" and "off" button named `buttonX_[off/on].png`, where X is the emote number which you will specify in the `char.ini` below. Icons should be 40x40.

The character icon is always named `char_icon.png` and is 60x60 in size.

You may also wish to customize the interjections, each of which have a fixed name:

- "Hold it!" (`holdit.wav`)
- "Take that!" (`takethat.wav`)
- "Objection!" (`objection.wav`)
- Custom (`custom.wav`)

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
      sadcustom.wav
```

Since WAV files are very large, the Ogg (`.ogg`) format may also be used for any sound effect or interjection. The audio codec may be Vorbis or Opus.

### 2. Creating a `char.ini`

What is ini-editing, exactly? Ini-editing is modifying a character's .ini file to change the way it interacts with the game client. Failed attempts at ini-editing may cause errors, which nobody likes. This guide aims to cover every aspect of ini-editing as in-depth as possible.

```ini
[Options]
name = Joe
side = wit

[Time]
slammin = <preanim_time>

[Emotions]
1 = <comment>#<preanim>#<emote>#<mod>

[SoundN]
1 = <sfx-name>

[SoundT]
1 = <sfx-delay>
```

#### `[Options]`

- `name`: specifies which folder to look for character assets, i.e. this should be named the same as the character folder. (Mischievious players can change this name to something else to use another character; this is called *ini-swapping.*)
- `showname` (optional): this name will appear on the nameplate whenever the character speaks, however it is fetched locally which means if you write `showname = FINIX RAIT`, only you will see it. You won't have to restart or change characters for this to take effect, like most other edits.
- `side`: modifies where in the courtroom the character appears. Valid options:
  - `def` - Defense
  - `pro` - Prosecution
  - `hld` - Helper defense
  - `hlp` - Helper prosecution
  - `jud` - Judge
  - `wit` - Witness
  - `jur` - Juror (since 2.6)
  - `sea` - Seance (since 2.6)

#### `[Time]`

Now for the `[Time]` section. This dictates the duration of pre-animations. Simply explained, it's how long it'll take for the character to start talking after pressing enter. The pre-animation gif will loop as long as the delay lasts. Each tick is 60 milliseconds (i.e. 2 = 120 milliseconds). Note that these as well are fetched locally, so you won't be able to set `deskslam = 9001` and potentially freeze a server.

#### `[Emotions]`

Onward to the `[Emotions]` section. This is where you configure what emotes your character has and how they work. If your character has more than 10 emotes, `firstmode =` will most likely be 10. Or else, they won't display properly as it tells the game client how many emotes will be on the first page.

The `number` option is pretty self-explanatory. It specifies the number of emotes.

Now for the specific emotions.

##### `<comment>`

Emote comments will display in the dropdown menu and on the emote button itself if an emote icon for it could not be found.

Some char.ini files might contain odd symbols; this is because the creator of Attorney Online (FanatSors) happened to be Russian and wrote many character files in Cyrillic, which Notepad doesn't read well.

##### `<preanim>`

The next section defines the preanimation, or the gif played before the character actually starts speaking. If there is none, a placeholder called `normal` or `-` is typically used.

##### `<emote>` (idle and speaking animations)

Between the next #'s is the name of the actual animation when the character is idle and speaking, starting with `(a)` and `(b)` in the character folder, respectively. For instance, if your animation is named "cough", then the engine expects your idle animation to be named `(a)cough.gif` on the file system, and your talking animation to be named `(b)cough.gif`.

The engine will also scan for other supported file formats, such as `.apng` and `.webp`, as described above.

If no animations are found for an emote, the engine will fall back to static emotes. Static emotes don't use `(a)`/`(b)` prefixes - instead, an static emote named `cough` would be expected to be in the file `cough.png`.

##### `<modifier>`

Finally, there's a numbered modifier. The valid inputs here are: 0, 1, and 5.

- `0`: Tells the client to not play the pre-animation or sound effect associated with the emote unless 'Take that'/'Objection'/'Hold it' is used.
- `1`: Plays the pre-animation and associated sound.
- `5`: Zoom, in which the foreground desk or witness stand will not be displayed. Additionally, the background is replaced by speed lines.

#### `[SoundN]`

Under `[SoundN]`, we see a list of numbers equalling something else. The leftmost number is the emotion number and the value to the right is the sound effect associated with that emotion. In many cases, there isn't one, and therefore the default sound `1.wav` is used, which is effectively nothing (since it doesn't actually exist). Other than that, we see for example emotion number seven being the desk slam, looking at the sound effect name.

#### `[SoundT]`

`[SoundT]` is essentially the delay before the sound effect is played. The only significant value here is `7 = 4`, which is once again the `deskslam` emote. As you probably know, the `deskslam` sound is only supposed to play when hands actually come in contact with the desk and not in the start of the animation; the '4' value makes sure of that. Again, each tick is 60 milliseconds.

### Sample INI file

```ini
[Options]
name = Phoenix
side = def

[Time]
deskslam = 10
nope = 20
objecting = 10
nodding = 16
ohshit = 24
phoenix-chugs = 30
sweating = 10

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
1 = 1
2 = 1
3 = 1
4 = 1
5 = 1
6 = 1
7 = sfx-deskslam
8 = 1
9 = sfx-stab2
10 = 1
11 = 1
12 = 1
13 = sfx-deskslam
14 = sfk-deskslam

[SoundT]
1 = 1
2 = 1
3 = 1
4 = 1
5 = 1
6 = 1
7 = 4
8 = 1
9 = 1
10 = 1
11 = 1
12 = 1
13 = 1
14 = 1
```

### Special quirks

#### Ordering

Putting `[Time]` after `[Emotions]`, for example, will not work. (TODO: is this still true?)

#### Preanims

There are certain issues with preanims (shorthand for preanimations) related to characters being specifically made for a certain timing in AO1. To properly explain how AO2 deals with this, let me introduce two terms; real and full time.

Real time is the time it would take for the gif to play once. I believe it depends on things like complexity, color depth and frame rate (gif quality, essentially). This varies drastically between AO1 and AO2. Notably, the HD ones play a lot slower than they "should" in AO 1.x (compared to, say, a web browser). This has caused content creators to speed them up, which creates this gap.

Full time is the number specified under the `[Time]` section multiplied by about 60 milliseconds. While there might be slight variations between 1.x and AO2, they remain largely the same.

First off, there is a common case especially in HD characters rips that they play a lot faster than intended. To make them behave somewhat sane, AO2 slows gifs that would play longer than the time specified in `[Time]` to match that time.

Now how do we make everything magically work? With mathemagic, of course.

If the Full time is longer than the Real time (that is, if the gif would loop and start over), the gif is "stretched"/slowed down to fit the full time. Also worth noting is that if Real time is longer than Full time, the gif will be cut short. At no point will AO2 speed up gifs. 

Putting `<preanim> = 0` in `[Time]` will make the gif play once at normal speed. (This is probably what you want.)

This is not perfect, but it makes gifs adjustable by just changing char.inis.

#### Talking and idle animations

The issue with too fast gifs is also present here, but has less of an impact (typically a character will blink and move their mouths a bit faster). Unfortunately, this has to be fixed manually.

### Distributing

Generally, characters are distributed in a bundle. A typical zip file looks like this:

```
base/
  characters/
    MyCharacter1/
      ...
    MyCharacter2/
      ...
  sounds/
    general/
      ...
README.txt
```

This reduces the cognitive load on the player, as all they have to do is drag and drop the contents of the zip file into their AO folder.

I believe that pretty much sums it up. Happy ini-editing!

---

*Much of this content was adapted from the* [Attorney Online User Manual](https://docs.google.com/document/d/1Si-d8lsJZla-BB0lhjDAwrUmawrRaMIf1EGaVNFEE_s/edit#) *and* [A comprehensive guide to ini-editing](https://docs.google.com/document/d/1q21JTx5ca28VsBFgE12MAEKHxTO6zyYfpYYd-nJfuVk/edit#heading=h.cpfyd4n0hpqp) *written by OmniTroid.*
