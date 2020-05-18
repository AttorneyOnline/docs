## Creating Backgrounds

Backgrounds are typically DS resolution (256x192). The `gs4` background is typically the default background, though the `default` folder acts as a fallback for missing desks.

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

### AO1 backgrounds

For the sake of documentation, it should be noted that some AO1-era backgrounds use different dimensions for desks.

Even though we previously stated that Fanat is Russian, these desks are named in Spanish (`bancodefensa`, `bancoacusacion`, `estrado`). Why? I don't know!

Instead of taking a full 256x192, these desks have a limited resolution, and the height of the viewport is also limited to just less than 192 pixels.

---

*Some of this content was adapted from the* [Attorney Online User Manual](https://docs.google.com/document/d/1Si-d8lsJZla-BB0lhjDAwrUmawrRaMIf1EGaVNFEE_s/edit#) *written by OmniTroid.*
