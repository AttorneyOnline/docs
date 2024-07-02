## Resizing
Resize modes are used to determine how content is meant to be scaled up/down. This is useful if your content is lower resolution and you want to keep the pixelated look or if you'd prefer to enforce a smoother look to the final image that is displayed within the viewport.

The following resize modes are available:
* `auto`: Automatically adjusted based on several conditions.
* `pixel` (aka `fast`): A pixelated look is retained when scaled.
* `smooth`: A smoother look is applied when scaled. (For example, edges may appear rounded.)

When resizing content to fit within the viewport, **if a predetermined resize mode is set to `auto` or is not specified**, a set of rules to determine what mode is used to provide the final image. The following rules apply:
* If content is set to stretch within the layer it belongs to or if the content is larger vertically than the layer it belongs to, the resize mode is changed to `smooth`.
  * If the above condition is not met, the resize mode is changed to `pixel`.
