# process-video

process-video is a collection of BASH format shell scripts that convert a directory of video files with inconsistent 
format and naming conventions to a single unified format and synchronise them to an output directory, possibly on a 
NAS.

The source directory is scanned for video files, any that don't exist in the destination directory will be transcoded to
an output format that has been tuned to the high profile, level 4.1, H.264 MPEG-4 with 1 Mb/s bitrate and 128kb/s stereo
AAC audio format supported by XBOX 360 and many similar generation consoles and players.

Videos are also renamed and retitled during the trascoding process using the following convention:

`/src/Movies/A.Movie.2001.1080p.mkv` would be titled "A Movie (2001)" and saved to `/dst/Movies/a-movie-2001-1080p.mp4`

`/src/Shows/Some.Show.S01E03.Episode.Title.1080p.mkv` would be titled "Some Show S01E03 Episode Title" and saved to
`dst/shows/Some Show/Some Show S01/some-show-s01e03-episode-title-1080p.mp4`

Both the destination directory and filename can be slugified independently by altering the slug setting in the config 
file. Slugification makes filenames a bit easier to deal with by converting them to lowercase, removing all punctuation
and replacing all spaces with dashes.

## Prerequisites
process-video makes use of ffmpeg and rsync to install these on a Debian based system use:
```sh
$ sudo apt install ffmpeg rsync
```

## Installation
no installation required just edit the config file to reflect your set-up then work from within the process-video 
directory.

## Examples
To transcode all new files and save them to the remote directory:
```shell
$ ./process-video
```
To transcode a single file and output it to the process-video directory:
```shell
$ ./process-one /path/to/video-file.mkv
```
process-video tries to determine the minimum amount of transcoding it can get away with to produce a compatible output 
file, if it gets this wrong and produces a file which isn't compatible with the target system you can force it to fully 
transcode the file by passing the forceEncode parameter like so:
```shell
$ ./process-video 1
$ ./process-one /path/to/video-file.mkv 1
```
If an output file is not named to your liking try editing the filename and running:
```shell
$ ./names /path/to/video-file.mkv
```
which will output the destination subdirectory, filename, title and whether the file is determined to be a subtitle file
or not. Once you are happy with the generated name, run process-video as normal to transcode the file.
