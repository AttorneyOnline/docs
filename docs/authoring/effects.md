## Creating Effects
Starting in AO 2.8.X, effects are added as an additional layer to the viewport, acting as a temporary overlay.

The content, without any reasonable consideration, is located in two places.
Default effects can be found in `base/themes/default/effects/` while character-specified effect folders can be found in `base/misc/<effects>/`
We are still unsure how this was ever considered a good organisation.

The internal folder structure is:
```
<folder>/
    effects.ini
    asset.extension
```
We could go on about the "v1" implementation of this system, but due to lack of functionality it never saw high adoption, so we will omit it.

Starting in AO 2.10.0 effects received a major overhaul to implement many baseline functionality users had been asking for.
Unlike other configurations like char.ini, the client will automatically migrate effects.ini files that it tries to load to the "v2" format.

An example of a correctly formatted design.ini is as follows:
```ini
[version]
major = 2

[0]
name = realization
sound = sfx-realization
cull = true
layer = chat
loop = false
max_duration = 60
respect_flip = false
respect_offset = false
scaling = smooth
sticky = false
stretch = true
```
A brief explanation follows:
The ini is versioned, unlike other AO2 configuration files:
- `Version`
  - `major`: Defines the major version of the effects.ini. Implemented to provide easier migration handling if future formats are introduced.<br>

This block is per-effect:
- `Index`: Used for sorting purposes. You don't want an unsorted effects list, do you? It needs to be a valid positive number.
  - `name`: The name of the effect, which also doubles as the effects filename.
  - `sound`: A sound to play for this effect
  - `cull`: Whether or not to delete the image once it's done playing. Ignored if loop=true
  - `layer`: Which layer it's going to be on. Possible options are:<br>
    - behind - behind the character<br>
    - character - over the character<br>
    - over - over everything in IC<br>
    - chat - over the chat box and other UI.<br>
    Due to a documentation error older docs incorrectly use `under` instead of `behind`<br>
  - `loop`: Whether the effect is looped or not. Overwrites culling to false if true.
  - `max_duration`: Maximum duration for this effect in milliseconds. Put at 0 for no maximum effect time.
  - `respect_flip`: Whether the character flip state is applied to the effect.
  - `respect_offset`: Whether the character offset is applied to the effect. 
  - `scaling`: The scaling algorithm used. `smooth` (bilinear) or `pixel` (nearest neighbour)
  - `sticky`: If true the effect stays selected in the dropdown.
  - `stretch`: Whether to stretch the effect across the viewport.
