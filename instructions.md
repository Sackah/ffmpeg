use the ffmpeg command followed by the (-i) flag to supply one or more input files. eg:
`ffmpeg -i in.mp4`

to convert the file to a different format, add the new format's extension to the output file. eg:
`ffmpeg -i in.mp4 out.mov`

the (-c) flag can me used to specify a specific codec. eg:
`ffmpeg -i in.mp4 -c:v mpeg4 -c:a mp3`
this creates an output file with an mpeg encoded video and mp3 encoded audio

the output quality of the file can be changed using many different flags.
bitrate (-b):
`ffmpeg -i in.mp4 -b:v 1000k out.mp4`
framerate (-r):
`ffmpeg -i in.mp4 -r 30 out.mp4`
resolution (-s):
`ffmpeg -i in.mp4 -r 30 -s 640x480 out.mp4`

in a case where you have multiple videos that you would like to combine, you can list them like in the vids.txt file, and specify the input by using concat and the codec as copy:
`ffmpeg -i vids.txt -f concat -c copy`

copy can also be used to make modifications to a video, in this case (-t) is used to trim some seconds off of a video footage
`ffmpeg -i in.mp4 -ss 00:00:30 -t 00:00:10 -c copy out.mp4`

(-vf) can be used to create a filter graph which can handle transformations like rotation, scaling and color modifications
`ffmpeg -i in.mp4 -vf scale=64:360`
`ffmpeg -i in.mp4 -vf eq=brightness=0.5`

if you have an srt file for captions in your video, ffmpeg is able to convert it into an ass file, which can then be used with the bf option to add subtitles to your video
`ffmpeg -i small.srt big.ass`

to change the quality of a video, you can use the (-q) flag for writing avi files and (-crf) for writing mp4 files.when using the q flag, you can add a number parameter known as a quantizer. the lower the number, the higher the quality and vice versa. typically you would want around 23^ for normal compression and up to 50 for extreme or single digits for higher quality:
`ffmpeg -i in.mp4 -q 23 out.avi`
`ffmpeg -i in.avi -crf 23 out.mp4`

ffmpeg has some inbuilt filters for working with audio and video

-   tweaking the volume: the number being passed is a multiplier, use decimals to reduce the volume
    `ffmpeg -i in.mp4 -filter:a "volume=2" out.mp4`
-   channel remaping: in this case we are mapping the input left (0) channel to the output left (0) channel and the input left (0) channel to the output right (1) channel. this converts a mono audio to a stereo
    `ffmpeg -i in.mp3 -filter:a "channelmap=0-0|0-1" out.mp3`
-   cropping: the first two parameters are the width and height, the last two are optional and specify the upper left corner of the cropping, if unspecified the cropping is centered
    `ffmpeg -i in.mp4 -filter:v "crop=w=640:h=480:x=100:y=200" out.mp4`
-   scaling: very similar to cropping and you can use arithmetic with provided variables in_w and in_h for the input file's width and height, and you can use -1 for proportional scaling of one of the parameters
    `ffmpeg -i in.mp4 -filter:v "scale=w=2/3*in_w:h=2/3*in_h" out.mp4`
    `ffmpeg -i in.mp4 -filter:v "crop=w=640:h=480:x=100:y=-1" out.mp4`
-   rotation: the angle needs to be clockwise and specified in radians. here we convert 45degrees to radians
    `ffmpeg -i in.mp4 -filter:v "rotate=45*PI/180" out.mp4`
