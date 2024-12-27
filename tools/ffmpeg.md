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

## Concat

```bash
> nano list.txt
file 'v_01.mp4'
file 'v_02.mp4'
file 'v_03.mp4'
>ffmpeg -safe 0  -f concat -i list.txt  -c copy cat.mp4
```

Fix some SAR issues:
```bash
>ffmpeg -i m_01.mp4  -vf "setsar=sar=1/1,setdar=dar=16:9" -aspect 640:360 -c:v libx264 -c:a aac  v_01.mp4
```

Concat v2:
```bash
> ffmpeg -i v_01.mp4 -i v_02.mp4 -i v_03.mp4 -filter_complex "[0:v][0:a][1:v][1:a][2:v][2:a]concat=n=3:v=1:a=1[v][a]" -vsync 2 -map "[v]" -map "[a]" out.mp4
```
