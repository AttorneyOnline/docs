## Creating Themes

*Note: This section is only applicable to desktop AO.*

The desktop AO client uses a custom UI loader that reads INI files which define the absolute position of every widget in the lobby and courtroom. This loader was developed by OmniTroid as a quick and dirty way to recreate the classic AO1 courtroom; however, it can be leveraged to develop custom themes for exotic resolutions and server branding.

Before considering a theme, you must first understand two main limitations of themes:

1. They break immediately on new versions, since new versions have buttons and widgets whose positions will not be defined by your (now outdated) theme. These new values will always be pulled from the Default theme.
2. They are *absolutely positioned*, meaning that they will not adapt to the size of the window.

These limitations will be addressed in a later version, which will replace the in-house theming system with Qt's native theming system.

### Minimum Contents

A theme is a folder in `base/themes`. On startup, the client scans all folders in `base/themes` and considers each directory to be a theme (regardless of the validity of its contents).

Technically, a theme has no minimum files. It can be empty; any files not in your theme are simply searched in the default theme instead. Therefore, it is not necessary to copy the entire default theme - you only need to copy the files you want to edit.

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
fontname = size
fontname_font = name
fontname_color = r, g, b
fontname_bold = 0/1
fontname_sharp = 0/1
```

AO2 only supports TrueType format (TTF) fonts. OpenType format (OTF) fonts will work, but have several issues (see [AO2-Client#224](https://github.com/AttorneyOnline/AO2-Client/issues/224)) making them look terrible in some cases. If the font you want to use is in OpenType format (OTF), this documentation recommends the use of [otf2ttf](https://pypi.org/project/otf2ttf/) to convert the font to TrueType format. 
***DO NOT USE ONLINE CONVERSION SITES.*** Many OTF fonts that exist have special properties that cause these web converters to fail, resulting in TFF files that are unusable. Please use the tool provided above instead.

### Qt Stylesheets

As of 2.8, you can customize the UI using [Qt CSS stylesheets](https://doc.qt.io/Qt-5/stylesheet-syntax.html), which allows significantly greater freedom in widget and font styles. In 2.9.1, further CSS support was added. Please see [Qt CSS Objects](https://github.com/Crystal2002/docs/blob/master/AO%20Documentation/docs/authoring/theme%20documentation/Qt%20CSS%20Objects.md) for the object names of each element of the client (the documentation assumes you already either know how to use Qt CSS Stylesheets, or you're a fast learner.)

### Sounds

Some default sounds can be customized in `courtroom_sounds.ini`. The syntax is as follows:

```ini
; must include correct extension (*.wav, *.opus)
event = sfx-name.wav
```

### Chatboxes

Prior to 2.8, the chatbox was governed solely by `chat.png`. Since 2.8, `chatblank`, `chatmed`, and `chatbig` are also used depending on the length of the showname. (`chatbox` can also be used if multiple chatbox images seems unnecessary.) These chatboxes can be overridden on a per-character basis, where the new chatbox can be found in its respective directory in `misc`.
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE3NjU2MTc5MDgsMTc0NDMxMDgzNV19
-->