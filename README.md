# FFMpeg-MP4Box

## All ffmpeg commands

### How to covert a file format
```
ffmpeg -i input.mp3 output.ogg
```
This command takes an MP3 file called input.mp3 and converts it into an OGG file called output.ogg. From FFmpeg's point of view, this means converting the MP3 audio stream into a Vorbis audio stream and wrapping this stream into an OGG container. You didn't have to specify stream or container types, because FFmpeg figured it out for you.

#### with videos:
```
ffmpeg -i input.mp4 output.webm
```
Because WebM is a well-defined format, FFmpeg automatically knows what video and audio it can support and will convert the streams to be a valid WebM file.

Depending on your container of choice, this won't always work. For instance, containers like Matroska are designed to handle almost any stream you care to put in them, whether they're valid or not. This means the command:
```
ffmpeg -i input.mp4 output.mkv
```
may result in a file with the same codecs as input.mp4 had, which may or may not be what you want.

### How to contact same types of file using concat demuxer:
Create a file mylist.txt with all the files you want to have concatenated in the following form (lines starting with a # are ignored):
```
# this is a comment
file '/path/to/file1.wav'
file '/path/to/file2.wav'
file '/path/to/file3.wav'
```
Note that these can be either relative or absolute paths. Then you can stream copy or re-encode your files:
```
ffmpeg -f concat -safe 0 -i mylist.txt -c output.mkv
```
The -safe 0 above is not required if the paths are relative.

### To extract audio stream 
```
 ffmpeg -i sample.mp4 -c:a copy -vn audio.mp4
```
### Commands to convert videos into different resolutions
```
ffmpeg -i sample.mp4 -an -c:v libx264 -x264opts 'keyint=24:min-keyint=24:no-scenecut' -b:v 5300k -maxrate 5300k -bufsize 2650k -vf 'scale=-1:1080' video-1080.mp4
ffmpeg -i sample.mp4 -an -c:v libx264 -x264opts 'keyint=24:min-keyint=24:no-scenecut' -b:v 2400k -maxrate 2400k -bufsize 1200k -vf 'scale=-1:720' video-720.mp4
ffmpeg -i sample.mp4 -an -c:v libx264 -x264opts 'keyint=24:min-keyint=24:no-scenecut' -b:v 1060k -maxrate 1060k -bufsize 530k -vf 'scale=-1:478' video-480.mp4
ffmpeg -i sample.mp4 -an -c:v libx264 -x264opts 'keyint=24:min-keyint=24:no-scenecut' -b:v 600k -maxrate 600k -bufsize 300k -vf 'scale=-1:360' video-360.mp4
ffmpeg -i sample.mp4 -an -c:v libx264 -x264opts 'keyint=24:min-keyint=24:no-scenecut' -b:v 260k -maxrate 260k -bufsize 130k -vf 'scale=-1:242' video-240.mp4
ffmpeg -i sample.mp4 -an -c:v libx264 -x264opts 'keyint=24:min-keyint=24:no-scenecut' -b:v 160k -maxrate 160k -bufsize 80k -vf 'scale=-1:144' video-144.mp4

```


## MP4BOX commands

### mp4Box command to dashify
```
MP4Box -dash 4000 -frag 4000 -rap -segment-name segment_%s -url-template -out final_dash.mpd output.mp4#video  output.mp4#audio

```

### mp4Box command to dashify video of different resolutions
```
MP4Box -dash 4000 -frag 4000 -rap -segment-name seg_%s -url-template -out sample_dash.mpd video-1080.mp4 video-720.mp4 video-480.mp4 video-360.mp4 video-240.mp4 video-144.mp4 audio.mp4
```
mp4box and mediaplayers do not accept multiplexed steams thus always seperate audi and video using ffmpeg.
















