# MS Packet Reference

MS is by far the most complex packet in the AO Standard and has a very large number of fields.

That is why it has its own page here.

Receivers: `Server, Client`

| Key                       | Type     | Rules                   |
|---------------------------|----------|-------------------------|
| `desk_mod`                | `number` | `0-5`                   |
| `preanim`                 | `number` |                         |
| `character`               | `string` | `0-5`                   |
| `emote`                   | `number` |                         |
| `message`                 | `string` |                         |
| `side`                    | `string` | Must be a valid side    |
| `sfx-name`                | `string` |                         |
| `emote_modifier`          | `number` |                         |
| `char_id`                 | `number` |                         |
| `sfx_delay`               | `number` |                         |
| `shout_modifier`          | `number` |                         |
| `evidence`                | `number` |                         |
| `flip`                    | `number` |                         |
| `realization`             | `number` |                         |
| `text_color`              | `number` |                         |
| `showname`                | `string` |                         |
| `other_charid`            | `number` |                         |
| `other_name`              | `number` | Not present from Client |
| `other_emote`             | `number` | Not present from Client |
| `self_offset`             | `number` |                         |
| `noninterrupting_preanim` | `number` |                         |
| `sfx_looping`             | `number` |                         |
| `screenshake`             | `number` |                         |
| `frames_shake`            | `number` |                         |
| `frames_realization`      | `number` |                         |
| `frames_sfx`              | `number` |                         |
| `additive`                | `number` |                         |
| `effect`                  | `number` |                         |
| `blips`                   | `number` |                         |

An in-character (IC) message is a basic form of viewport event in which a animation is displayed on the screen with various parameters. Line breaks are included for cleanliness and are not present in the actual packet.

> Note that the only difference between the client and server messages is that the client does not have the `other_name` or `other_emote` fields.

Reference of fields:

- `desk_mod`: Whether or not to override desk appearance.
  -`chat`: Positions "def", "pro", and "wit" default to desk and the positions "hld", "hlp" and "jud" to no desk.
  -`0`: desk is hidden
  - `1`: desk is shown
  - `2`: desk is hidden during preanim, shown when it ends
  - `3`: desk is shown during preanim, hidden when it ends
  - `4`: desk is hidden during preanim, character is centered and pairing is ignored, when it ends desk is shown and pairing is restored
  - `5`: desk is shown during preanim, when it ends character is centered and pairing is ignored

- `preanim`: The animation that plays before the character starts talking. Does not include file extension.

- `character`: The folder name of the character that is talking.

- `emote`: The emote that should play. Does not include `(a)`/`(b)` prefixes or file extensions.

- `message`: The chat message as to be displayed in the chatbox and the IC log. Note that the message may contain markup that must be parsed.

- `side`: Which side the character is on. See the character authoring page for valid positions.

- `sfx_name`: Name of the sound effect that should play during the preanimation (if the preanimation is enabled).

- `emote_modifier`: A number that dictates emote behavior:
  - `0`: do not play preanimation; overridden to 2 by a non-`0` objection modifier
  - `1`: play preanimation (and sfx)
  - `2`: play preanimation and play objection
  - `3`: _unused_
  - `4`: _unused_
  - `5`: no preanimation and zoom
  - `6`: objection and zoom, no preanim

- `char_id`: Character identifier; dictates the index of the character in the character list (starting from zero).

- `sfx_delay`: Dictates how long in milliseconds the client should wait after the preanimation has started playing before playing the associated sound effect.

- `objection_modifier`: Dictates if the player uses a shout.
  - `0`: nothing
  - `1`: "Hold it!"
  - `2`: "Objection!"
  - `3`: "Take that!"
  - `4&{name}`: custom shout (since 2.8)

- `evidence`: ID of the evidence presented. 0 is no evidence presented, so evidence ID effectively starts from 1.

- `flip`: Dictates if the emote should be flipped.
  - `0`: no flip
  - `1`: flip

- `realization`: Dictates whether a realization flash and sound effect should play or not.
  - `0`: no realization
  - `1`: realization

- `text_color`: Dictates which color the text of the chat message should be.
  - `0`: white
  - `1`: green
  - `2`: red
  - `3`: orange
  - `4`: blue (disables talking animation)
  - `5`: yellow
  - `6`: rainbow (removed in 2.8)

- `showname`: If used, this will show a custom name (showname) for the character.

- `other_charid`: The character ID of the person the player wishes to pair up with.

- `other_name`: The folder name of the character the player is pairing up with (in case of INI swapping).

- `other_emote`: The emote the user's pair was doing. Note that by default, zooms (that are correctly defined as such) do not update this value, so a pair of zooms will not appear. Zooms also enjoy special privileges, in that (assuming they are correctly defined, again) they make the pair disappear in the client and get centered.

- `self_offset`: the percentage by which the character is shifted horizontally, from `-100` (one whole screen's worth to the left) to `100` (one whole screen's worth to the right). This parameter also stores vertical offset, which is self-explanatory.
  - `{x_offset}`: <2.9
  - `{x_offset}&{y_offset}`: 2.9+

- `other_offset`: The user's pair's `self_offset`, basically.
- `other_flip`: The user's pair's `char_id2/flip`, basically.
- `noninterrupting_preanim`: If `1`, the text begins at the same time as the preanimation.
- `sfx_looping`: If `1`, the sound effect loops until another emote is played.
- `screenshake`: If `1`, the screen shakes (TODO: on preanimation or on chat?).
- `frames_shake`: A list of frames for which the screen should shake (TODO: list format).
- `frames_realization`: A list of frames for which the screen should flash (TODO: list format).
- `frames_sfx`: A list of frames for which the sound effect should play.
- `additive`: If `1`, does not clear the chatbox's previous message.
- `effect`: The overlay effect to be displayed.
- `blips`: The sound effect to play while processing text, colloquially the "blips."

All sections from `showname` onwards is 2.6+. `sfx_looping` onwards is 2.8+. `blips` onward is 2.10.2+.

> Note that the Server may freely modify any value of this message. For example, disemvoweling and shaking modify your text, and `/force_nonint_pres` forces your preanims to be noninterrupting. Therefore, to determine if your message was successfully sent, the character ID should be compared instead of the message text or the entire packet.
