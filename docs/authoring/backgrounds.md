## Creating Backgrounds
### AO2 Backgrounds (2.9.x+)

All of AO Vanilla's backgrounds are DS resolution (256x192), although you will typically come across backgrounds with either a 4:3 aspect ratio or a 16:9 aspect ratio.

Starting in AO 2.8.x, backgrounds can be PNG/GIF/WebP and will support animations (such as the rushing water in 999's Third Class Cabin).

Starting in AO 2.9.x, backgrounds no longer have to follow the default naming system (as seen in the next section). Overlays can be applied by adding the suffix `_overlay` to the filename (i.e. - `Cabin_overlay.png`).

Additionally, since 2.9 backgrounds can have **custom positions**. The images for these positions should be the name of the position, and the overlay should be the name of the position with the suffix `\_overlay`. These will only appear in the positions dropdown if they are added to a file called `design.ini` in the background's folder. An example of a correctly formatted design.ini is as follows:

```ini
scaling = pixel
positions = pos1,pos2,pos3,pos4
judges = pos2,pos4
```
A brief explanation follows:

- `scaling`: The resize mode used. For more information, see [Viewport](viewport.md)
- `positions`: A list of all the custom positions for this background, separated by commas.
- `judges`: (since 2.9.1) A list of all the positions for this background that should have access to the judge controls. Note that "jud" will always have access to the controls regardless of this setting.

If the background is "AO2-compatible" (read: contains `stand`, `defensedesk` and `prosecutiondesk`), the position of the chat bar and chat box are fetched from `ao2_ic_chat_message` and `ao2_chatbox` in courtroom_design.ini, respectively.

### AO2 Backgrounds (Pre-2.9)

Prior to 2.9.x, the `gs4` background is typically the default background, though the `default` folder acts as a fallback for missing desks.

Backgrounds typically contain the following files:

- `defensedesk.png`
- `defenseempty.png`
- `helperstand.png`
- `judgestand.png`
- `prohelperstand.png`
- `prosecutiondesk.png`
- `prosecutorempty.png`
- `stand.png`
- `witnessempty.png`

If the background is AO2-compatible (read: contains `stand.png`, `defensedesk.png` and `prosecutiondesk.png`), the position of the chat bar and chat box are fetched from `ao2_ic_chat_message` and `ao2_chatbox` in courtroom_design.ini, respectively.

As of Client Version 2.8, you can define custom positions through `design.ini`.

### Slide-ready backgrounds (2.10.2+)

To set up backgrounds for use with slide transitions, first acquire a suitable image. These images are typically wide and have multiple "positions" included in them - All 2D Ace Attorney games have these.

Place the image and its overlay (if it has one) in the background's folder like any normal position. If it's a courtroom, you should name it `court.png` - this will enable the client to automatically use it for legacy positions (def, wit, pro).

Define each position in `design.ini` as follows:

```
[image:pos]
rect=x, y, w, h
slide_ms_pos3=550
slide_ms_pos2=500
```
- `image` - the name of the image, with no extension i.e. `court` for `court.png`
- `pos` - the name of the position, i.e. `def`
- `rect` - a rectangle around the part of the image you want this position to occupy. You can think of this like a rectangular cut-out of the full image.
  - `x` - the x coordinate of the top left corner of the rectangle
  - `y` - the y coordinate of the top left corner of the rectangle (this should generally be 0)
  - `w` - the width of the rectangle
  - `h` - the height of the rectangle
- `slide_ms_<pos2>` - the duration of the slide animation between `pos` and `pos2`, in milliseconds

If you're using `court.png`, the three positions you should define are `court:def`, `court:wit`, and `court:pro`, as the client will automatically translate these for you. The vanilla content base should contain an already-created background for you to reference.

### AO1 backgrounds

For the sake of documentation, it should be noted that some AO1-era backgrounds use different dimensions for desks. These desks' filenames are in Spanish (`bancodefensa`, `bancoacusacion`, `estrado`). (Why are they in Spanish if Fanat is Russian? I don't know!)

Instead of taking a full 256x192, these desks have a limited resolution, and the height of the viewport is also limited to just less than 192 pixels.

Support for these AO1-style desks was dropped in 2.8.

---

*Some of this content was adapted from the* [Attorney Online User Manual](https://docs.google.com/document/d/1Si-d8lsJZla-BB0lhjDAwrUmawrRaMIf1EGaVNFEE_s/edit#) *written by OmniTroid.*
*Credits to the AO2 Development Team for the original documentation*
