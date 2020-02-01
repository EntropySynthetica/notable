---
tags: [Linux]
title: Youtube-DL
created: '2020-01-30T20:16:16.043Z'
modified: '2020-01-30T21:04:28.358Z'
---

# Youtube-DL
Created Tuesday 13 December 2016

To install it right away for all UNIX users (Linux, OS X, etc.), type:
sudo curl -L <https://yt-dl.org/downloads/latest/youtube-dl> -o /usr/local/bin/youtube-dl

sudo chmod a+rx /usr/local/bin/youtube-dl

Update App
sudo /usr/local/bin/youtube-dl -U

Youtube download to MP3 example

youtube-dl --extract-audio --audio-format mp3 <https://www.youtube.com/watch?v=EP1mczaq3zo>

