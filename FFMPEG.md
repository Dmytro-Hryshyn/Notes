## FFMPEG

FFmpeg is a free and open-source software project consisting of a large suite of libraries and programs for handling video, audio, and other multimedia files and streams. At its core is the FFmpeg program itself, designed for command-line-based processing of video and audio files.

### Convert mkv to mp4

##### **Single File**

```sh
ffmpeg -i example.mkv -c copy example.mp4
```

##### Batch conversion

**Unix/Linux**

   ```sh
   for f in *.mkv; do ffmpeg -i "$f" -c copy "${f%.mkv}.mp4"; done
   ```

   **Windows**

```shell
for /R %f IN (*.mkv) DO ffmpeg -i "%f" -c copy "%~nf.mp4"
```

**Split Mp3 file**

```sh
# start time to end time:
ffmpeg -i input.mp3 -vn -acodec copy -ss 00:00:00 -to 00:01:32 output.mp3

# start time + duration time:
ffmpeg -i input.mp3 -vn -acodec copy -ss 00:00:00 -t 00:01:32 output.mp3

# start of record till end time:
ffmpeg -i input.mp3 -vn -acodec copy -to 00:01:32 output.mp3

# start time till end of record:
ffmpeg -i input.mp3 -vn -acodec copy -ss 00:00:00 output.mp3
```

**Split video file**

```sh

ffmpeg -i input.mp4 -ss 00:00:00 -to 00:10:00 -c copy output1.mp4

#-i input file
#-ss start time in seconds or in hh:mm:ss
#-to end time in seconds or in hh:mm:ss
#-c codec to use
```

