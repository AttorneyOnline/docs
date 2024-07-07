### Music

Music tracks are located in `base/sounds/music` and can be `.mp3`, `.wav`, `.ogg`, or `.opus`. Transcoding music to Opus at a bitrate of 64-96k is recommended to save space at extremely minimal quality loss.

Starting in Client Version 2.8.0, you can create seamless A-B loops in tracks. Create a config file with the same name as the song, including the extension, ending in `.txt` (e.g. `song.opus.txt`). The configuration must have `loop_start` and either `loop_length` or `loop_end`. Previously to client version 2.10.2, values were in "samples" - however the preferred method is now seconds down to the third decimal point. To use this preferred mode, the first line of the ini must be `seconds=true` - for compatibility, if this is not present the values will be read as samples instead.

Example `.txt` file:
```ini
seconds=true
loop_start=4.612
loop_end=64.905
```
