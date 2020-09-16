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

