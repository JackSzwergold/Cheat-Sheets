---
Title: Find - Video Processing Items
Description: A cheat sheet for Find related video processing items.
Author: Jack Szwergold
Date: 2017-07-25
Robots: noindex,nofollow
Template: index
---

### Video Metadata Parser

This script (`video_metadata_parser.sh`) uses ExifTool to parse dates, times and titles from a directory of video files on the desktop. Info is saved on the desktop into a dated text file named something like `VideoInfo-2017-07-25.txt`:

    find -E "Desktop/Videos" -type f -iregex ".*\.(M4V|MOV)$" |\
      while read full_video_filepath
      do

        # Break up the full audio filepath stuff into different directory and filename components.
        video_dirname=$(dirname "${full_video_filepath}");
        video_basename=$(basename "${full_video_filepath}");
        video_filename="${video_basename%.*}";
        video_extension="${video_basename##*.}";

        # Get the video metadata.
        video_date=$(exiftool -s -s -s -CreationDate "${full_video_filepath}");
        video_title=$(exiftool -s -s -s -Title "${full_video_filepath}");

        # Reformat the file creation date to be Flickr friendly.
        if [ ! -z "${video_date}" ]; then
          video_date_formatted=$(date -jf"%Y:%m:%d %k:%M:%S" "${video_date%-*}" +"%m/%d/%Y %k:%M:%S");
        else
          video_date_formatted="";
        fi

        # Reformat the file creation date to be Flickr friendly.
        echo "${video_filename} | ${video_date_formatted} | ${video_title}" >> "Desktop/VideoInfo-$(date +%Y-%m-%d).txt";

      done

### Extract MP4 Videos Out of an MKV File

Simple script traverse a directory filled with MKV files and extract the MP4 inside without transcoding:

    find -E 'Desktop/Movies' -type f -iregex '.*\.(MKV)$' |\
      while read FULL_PATH
      do
        PATH_SANS_EXTENSION="${FULL_PATH%.*}"
        ffmpeg -i "${FULL_PATH}" -codec copy "${PATH_SANS_EXTENSION}".mp4
      done

### Extract ASS Subtitles Out of an MKV File

Simple script traverse a directory filled with MKV files and extract the ASS formatted subitles out of them:

    find -E 'Desktop/Movies' -type f -iregex '.*\.(MKV)$' |\
      while read FULL_PATH
      do
        PATH_SANS_EXTENSION="${FULL_PATH%.*}"
        ffmpeg -y -v quiet -i "${FULL_PATH}" -codec:s ass -c copy "${PATH_SANS_EXTENSION}".ass & task_pid=(`jobs -l | awk '{print $2}'`);
        wait ${task_pid};
      done

### Add SRT Subtitles to MP4 Files

Simple script traverse a directory filled with MKV files and extract the ASS formatted subitles out of them:

    find -E 'Desktop/Movies' -type f -iregex '.*\.(MP4)$' |\
      while read FULL_PATH
      do
        PATH_SANS_EXTENSION="${FULL_PATH%.*}"
        ffmpeg -y -v quiet -i "${FULL_PATH}" -i "${PATH_SANS_EXTENSION}".srt -c:v copy -c:a copy -c:s mov_text -metadata:s:s:0 language=eng "${PATH_SANS_EXTENSION}".srt.mp4 & task_pid=(`jobs -l | awk '{print $2}'`);
        wait ${task_pid};
      done    

### Batch Convert MOV and DV files to an x265 MP4

Simple script traverse a directory filled with MOV and/or DV files and convert the contents to an x265 MP4 at a height of 480 pixels:

    find -E 'Desktop/Movies' -type f -iregex '.*\.(DV|MOV|MKV)$' |\
      while read FULL_PATH
      do
        PATH_SANS_EXTENSION="${FULL_PATH%.*}"
        caffeinate ffmpeg -y -v quiet -i "${FULL_PATH}" -map_metadata -1 -vf scale=-2:480 -c:v libx265 -x265-params log-level=error -crf 20 -c:a aac -b:a 128k -ac 2 -vol 384 -tag:v hvc1 -sn "${PATH_SANS_EXTENSION}".mp4  < /dev/null;
      done

### Batch Extract MP4 Video From Video Files

Simple script traverse a directory filled with DV, MOV and MKV files and extract the audio and video into a new file. Useful in cases wher Handbrake adds excessive duplicate frames; FFmpeg seems to be smart enought drop them:

    find -E 'Desktop/Movies' -type f -iregex '.*\.(DV|MKV|MOV|MP4)$' |\
      while read FULL_PATH
      do
        PATH_SANS_EXTENSION="${FULL_PATH%.*}"
        caffeinate ffmpeg -y -v quiet -i "${FULL_PATH}" -map_metadata -1 -c:v copy -c:a copy -map 0:0 -map 0:1 "${PATH_SANS_EXTENSION}"_content.mp4 < /dev/null;
      done

### Strip Metadata Out of Video Files

Simple script traverse a directory filled with MKV or MP4 files strips the metadata out of them:

    find -E 'Desktop/Movies' -type f -iregex '.*\.(MKV|MP4)$' |\
      while read full_video_filepath
      do

        # Break up the full audio filepath stuff into different directory and filename components.
        video_dirname=$(dirname "${full_video_filepath}");
        video_basename=$(basename "${full_video_filepath}");
        video_filename="${video_basename%.*}";
        video_extension="${video_basename##*.}";

        # Run the command.
        ffmpeg -y -v quiet -i "${full_video_filepath}" -map_metadata -1 -map 0 -c:v copy -c:a copy -c:s copy "${video_dirname}"/"${video_filename}"_new."${video_extension}" & task_pid=(`jobs -l | awk '{print $2}'`);
        wait ${task_pid};

      done
  
