# FFMPEG

Some doc :
 - https://img.ly/blog/ultimate-guide-to-ffmpeg/


## Cut without reencoding :

```bash
ffmpeg  -i source.mp4  -ss "0:12:00" -to "22:44"  -c copy output.mp4
```