## Creating Themes

*Note: This section is only applicable to desktop AO post 2.10.*

The desktop AO client uses Qt's UI loader that reads UI files from RCC ressources files which define layout of every widget in the lobby.

Before considering a theme, you must first understand three main limitations of UI themes:

1. They break immediately on newer versions of the client when required widgets are added, those being buttons or text areas the client interacts with.
Not having those included in your .UI file will cause the client to crash when trying to load it.
2. Subthemes are only partially supported as of writing.
3. Themes are loaded from an Ressource File, which require Qt's RCC toolchain.

These limitations will be addressed in a later client version. For those who want to ensure compatibility when updates occurs, please make sure to back up the *original assets* used to make your theme - The more you save, the less work that will have to be done later on!

### Minimum Contents

A theme is a Ressource File (.rcc) in `base/themes`. On startup, the client scans `base/themes` and considers each Ressource File to be a theme (regardless of the validity of its contents).

Technically, a theme has no minimum files. It can be empty; any files not in your theme are simply loaded from the embeeded executable. Therefore, it is not necessary to copy the entire default theme - you only need to copy the files you want to edit. The client pulls any "missing files" from the the executable.

Since UI files do not support comments, be sure to have read the Qt Designer Documentation!

### Fonts

AO2 only supports TrueType format (TTF) fonts. OpenType format (OTF) fonts will work, but have several issues (see [AO2-Client#224](https://github.com/AttorneyOnline/AO2-Client/issues/224)) making them look terrible in some cases. If the font you want to use is in OpenType format (OTF), this documentation recommends the use of [otf2ttf](https://pypi.org/project/otf2ttf/) to convert the font to TrueType format. 
***DO NOT USE ONLINE CONVERSION SITES.*** Many OTF fonts that exist have special properties that cause these web converters to fail, resulting in TFF files that are unusable. Please use the tool provided above instead.

### Qt Stylesheets

As part of the UI file [Qt CSS stylesheets](https://doc.qt.io/Qt-5/stylesheet-syntax.html), which allows significantly greater freedom in widget and font styles can be applied to any widget in the hierarchy.

As of 2.10.1, you can customize the UI using [Qt CSS stylesheets](https://doc.qt.io/Qt-5/stylesheet-syntax.html), which allows significantly greater freedom in widget and font styles. Please see [Qt CSS Objects](https://github.com/Crystal2002/docs/blob/master/AO%20Documentation/docs/authoring/theme%20documentation/Qt%20CSS%20Objects.md) for the required object names and type of each UI file of the client (the documentation assumes you already either know how to use [Designer](https://doc.qt.io/qt-5/qtdesigner-manual.html), or you're a fast learner.)