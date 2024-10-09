## Creating Themes

*Note: This section is only applicable to desktop AO.*

The desktop AO client uses a custom UI loader that reads INI files which define the absolute position of every widget in the lobby and courtroom. This loader was developed by OmniTroid as a quick and dirty way to recreate the classic AO1 courtroom; however, it can be leveraged to develop custom themes for exotic resolutions and server branding.

Before considering a theme, you must first understand two main limitations of themes:

1. They break immediately on newer versions of the client, as there will be buttons and widgets whose positions will not be defined by your (now outdated) theme. These new values will always be pulled from the Default theme.
2. They are *absolutely positioned*, meaning that they will not adapt to the size of the window.

These limitations will be addressed in a later client version, which will replace the in-house theming system with Qt's native theming system. For those who want to ensure compatibility when the replacement occurs, please make sure to back up the *original assets* used to make your theme - The more you save, the less work that will have to be done later on!

### Minimum Contents

A theme is a folder in `base/themes`. On startup, the client scans all folders in `base/themes` and considers each directory to be a theme (regardless of the validity of its contents).

Technically, a theme has no minimum files. It can be empty; any files not in your theme are simply searched in the default theme instead. Therefore, it is not necessary to copy the entire default theme - you only need to copy the files you want to edit. The client pulls any "missing files"  from `base/themes/default`.

Since the theme configuration files have a large amount of options and widgets, we hope that inline comments in the INI files will suffice in lieu of documentation here.

### Positioning

Widget positions are defined in `courtroom_design.ini` and `lobby_design.ini`. The syntax for the INI file is as follows:

```ini
; comment
widget_name = x, y, width, height
```

### Fonts

Fonts are defined in `courtroom_fonts.ini` and `lobby_fonts.ini`, and are highly recommended to be placed into the `base\fonts` folder for distribution. The syntax for the INI file is as follows:

```ini
element = fontsize
element_font = name
element_color = r, g, b
element_bold = 0/1
element_sharp = 0/1
```

AO2 only supports TrueType format (TTF) fonts. OpenType format (OTF) fonts will work, but have several issues (see [AO2-Client#224](https://github.com/AttorneyOnline/AO2-Client/issues/224)) making them look terrible in some cases. If the font you want to use is in OpenType format (OTF), this documentation recommends the use of [otf2ttf](https://pypi.org/project/otf2ttf/) to convert the font to TrueType format. 
***DO NOT USE ONLINE CONVERSION SITES.*** Many OTF fonts that exist have special properties that cause these web converters to fail, resulting in TFF files that are unusable. Please use the tool provided above instead.

For the `showname` element, special code allows for outlined text. This requires extra configuration:
```ini
showname_outlined = 0/1
showname_outline_color = r, g, b,
showname_outline_width = int (e.g. 3)
```

### Qt Stylesheets

As of 2.8, you can customize the UI using [Qt CSS stylesheets](https://doc.qt.io/Qt-5/stylesheet-syntax.html), which allows significantly greater freedom in widget and font styles. In 2.9.1, further CSS support was added. Please see [Qt CSS Objects](https://github.com/Crystal2002/docs/blob/master/AO%20Documentation/docs/authoring/theme%20documentation/Qt%20CSS%20Objects.md) for the object names of each element of the client (the documentation assumes you already either know how to use Qt CSS Stylesheets, or you're a fast learner.)

### Sounds

Some default sounds can be customized in `courtroom_sounds.ini`. The syntax is as follows:

```ini
; must include correct extension (*.wav, *.opus) in version 2.8.4 and older
event = sfx-name
```

### Chatboxes

Prior to 2.8, the chatbox was governed solely by `chat.png`. Since 2.8, `chatblank`, `chatmed`, and `chatbig` are also used depending on the length of the showname. (`chatbox` can also be used if multiple chatbox images seems unnecessary.) These can be overriden on a per-character basis, please see the [Character Documentation](https://github.com/Crystal2002/docs/blob/master/AO%20Documentation/docs/authoring/characters.md) for more details on this override.

Note that you may override the layout settings of custom chatboxes to better suit your theme. Custom chatboxes are generally located under `base/misc`. If you have a version under `[theme]/misc`, it will be used instead. Custom chatboxes have layout overrides via a `courtroom_design.ini` placed in their folder. Only chatbox-related settings may be overridden in this way.

To use the nameplate expanding functionality, create a base `chat.png` with a nameplate of reasonable length. Then, create three variants of it:

- `chatblank.png` - No nameplate
- `chatmed.png` - `chat.png`, but with the nameplate extended by a certain number of pixels **(write this down!)**
- `chatbig.png` - `chat.png`, but with the nameplate extended by exactly twice the amount of pixels you used for `chatmed`.

Lastly, set `showname_extra_width` to the amount of pixels you extended the nameplate by. Then, you can set up the chatbox's coordinates for `chat.png`, and they will be adjusted for `chatmed` and `chatbig` automagically.

### Theme Inheritance

Added in 2.9.0, it's now possible for themes to inherit elements from other themes. These are done by way of `Subthemes` and `Parent Themes`.

#### Subthemes

You can add a folder inside of a theme that can replace anything inside the existing theme. Subthemes are prioritized over your current theme and work like a third layer of inheritance, so the first layer is default theme, second layer is current theme and the last layer is the subtheme. You can manually force your subtheme using the settings screen, set it to "default" to stop subthemes from changing, or "server" to allow the server to suggest which subtheme to use (meaning you can have themes for day/night cycles, different interfaces between investigation/trial segments, etc.)

For example, the fallback order for  `courtroom_design.ini`  will be as follows:

-   `base/theme/subtheme/courtroom_design.ini`
-   `base/theme/courtroom_design.ini`
-   `base/theme/default_theme/courtroom_design.ini`

Note that subthemes will never fall back to any subthemes belonging to the default theme.

As of 2.9.1, the client makes no attempts to differentiate folders for the main theme (such as the `effects` folder) from subtheme folders.

#### Parent Themes
You can specify a new default theme for themes, so instead of falling back to the theme called `default`, you can fall back to any theme of your choice. To do this, add a `default_theme=` value in `courtroom_design.ini`. Note that it is not recursive, so if the default theme you selected has a `default_theme=` value as well, it won't fall back a second time, nor will it fall back to `default`.

### Animated UI
Starting in 2.9.0, it is possible to have elements of your UI be animated. To take advantage of this, make sure that you have `Animated Themes` checked in your settings, and use an animated format (WebP, APNG, or GIF) with your animated image. ***Please be aware that an overabundance of animated elements may impact AO's performance.***

### Stickers
Characters can now have an additional portrait displayed over the chatbox when they are talking.

To use this feature, place your sticker in a directory named  `sticker`  inside of your theme's folder. The sticker image must be named after the folder name of the character, and the image must be the size of the viewport.
