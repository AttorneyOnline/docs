## Creating Backgrounds

Backgrounds are typically DS resolution (256x192) though they can be any size (and they can even be animated, since 2.8). If the background is missing, the default background will be used (named `default`).

Pre-2.9 backgrounds typically contain the following files, corresponding to positions:

- `defensedesk`
- `defenseempty` ("def)
- `helperstand` ("hld")
- `judgedesk` (since 2.6)
- `judgestand` ("jud")
- `prohelperstand` ("hlp")
- `prosecutiondesk`
- `prosecutorempty` ("pro")
- `stand`
- `witnessempty` ("wit")
- `jurydesk` (since 2.6)
- `jurystand` ("jur") (since 2.6)
- `seancestand` ("sea") (since 2.6)

However, since 2.9, background filenames can be simplifed to simply the name of the position (and for the overlay, the name suffixed with "\_overlay") For example, `wit` and `wit_overlay`.

Additionally, since 2.9 backgrounds can have **custom positions**. The images for these positions should be the name of the position, and the overlay should be the name of the position with the suffix "\_overlay". These will only appear in the positions dropdown if they are added to a file called "design.ini" in the background's folder. An example of a correctly formatted design.ini is as follows:

```ini
scaling = smooth
positions = pos1,pos2,pos3,pos4
judges = pos2,pos4
```
A brief explanation follows:

- `scaling`: Defines the type of scaling to be use when upscaling this background. Default is `fast`, for nearest-neighbor. The only other option is `smooth`.
- `positions`: A list of all the custom positions for this background, separated by commas.
- `judges`: (since 2.9.1) A list of all the positions for this background that should have access to the judge controls. Note that "jud" will always have access to the controls regardless of this setting.

If the background is "AO2-compatible" (read: contains `stand`, `defensedesk` and `prosecutiondesk`), the position of the chat bar and chat box are fetched from `ao2_ic_chat_message` and `ao2_chatbox` in courtroom_design.ini, respectively.

### AO1 backgrounds

For the sake of documentation, it should be noted that some AO1-era backgrounds use different dimensions for desks. These desks' filenames are in Spanish (`bancodefensa`, `bancoacusacion`, `estrado`). (Why are they in Spanish if Fanat is Russian? I don't know!)

Instead of taking a full 256x192, these desks have a limited resolution, and the height of the viewport is also limited to just less than 192 pixels.

Support for these AO1-style desks was dropped in 2.8.

---

*Some of this content was adapted from the* [Attorney Online User Manual](https://docs.google.com/document/d/1Si-d8lsJZla-BB0lhjDAwrUmawrRaMIf1EGaVNFEE_s/edit#) *written by OmniTroid.*
