# FFMPEG

Some doc :
 - https://img.ly/blog/ultimate-guide-to-ffmpeg/


## Cut without reencoding :

```bash
ffmpeg  -i source.mp4  -ss "0:12:00" -to "22:44"  -c copy output.mp4
```


## Delay audio or video

Delay whole video with `itsoffset` then either use the delayed video : `-map 1:v -map 0:a` or the delayed audio : `-map 0:v -map 1:a`

```bash
ffmpeg -i movie.mp4 -itsoffset 3.84 -i movie.mp4 -map 1:v -map 0:a -c copy movie-video-delayed.mp4
```

## Repair a video

Simply make a copy of it :

```bash
>ffmpeg -i toto.mp4 -c copy toto-repaired.mp4
```
