### Music

Music tracks are located in `base/sounds/music` and can be `.mp3`, `.wav`, `.ogg`, or `.opus`. Transcoding music to Opus at a bitrate of 64-96k is recommended to save space at extremely minimal quality loss.

Starting in Client Version 2.8.0, you can create seamless A-B loops in tracks. Create a config file with the same name as the song, including the extension, ending in `.txt` (e.g. `song.opus.txt`). The configuration must have `loop_start` and either `loop_length` or `loop_end`. All time values are in terms of samples (use Audacity to convert seconds to samples).

Example `.txt` file:
```ini
loop_start=529577
loop_end=10038805
```