---
layout: post
title: FFMPEG Usage
image: /img/hello_world.jpeg
---

This post is about usage of ffmpeg. 
How to start using it from a basic level and can we use it in production environment?

1. Basic level Usage

Steps :--

1. We can download ffmpeg from here https://ffmpeg.org/download.html
2. Easiest way is to install static build, Based on our environments, we can choose win,mac or linux based static builds
3. Just download static build and unzip it. We are ready to use ffmpeg now.
4. We can add extracted folder path to PATH variable, so that every time we don't need to go to folder for running ffmpeg.




Various FFMPEG commands

1. Clipping the Video
{% highlight sh linenos %}
ffmpeg -ss starttime -i source_video.mp4 -to duration -q:a 0 -q:v 0 output_video.mp4
{% endhighlight %}


This above command will clip the video from starttime specified to duration of video.
You can get output video in any format you like, just enter specific format of output video.

~~~
-q:a 0
~~~
This is for specified for 0 quality loss for audio. Resulting file size will be bigger.

~~~
-q:v 0
~~~
This is specified for 0 quality loss for video.

2. Apply Fade in Fade out
{% highlight sh linenos %}
ffmpeg -i source_video.mp4 -sseof -1 -copyts -i source_video.mp4 -filter_complex "[1]fade=out:0:30[t];[0][t]overlay,fade=in:0:30[v]; anullsrc,atrim=0:1[at]acrossfade=d=1,afade=d=1[a]" -map "[v]" -map "[a]" -q:v 0 -q:a 0 output_video.mp4
{% endhighlight %}

This will apply fade in and fade out for starting 30 frames and ending 30 frames of a video.
You can play around with frame nos.


3. Convert Mp4 to ts format

We shouldn't merge two mp4 files as such. We should convert them to ts format first and then merge them.
Otherwise we will see lot of issues like audio sync issue etc.

{% highlight sh linenos %}
ffmpeg -i source_video.mp4 -c copy -bsf:v h264_mp4toannexb -f mpegts intermediate.ts
{% endhighlight %}


4. Merge Ts files and convert to mp4 format

{% highlight sh linenos %}
ffmpeg -f concat -safe 0 -i file.txt -q:a 0 -q:v 0 -bsf:a aac_adtstoasc output_video.mp4
{% endhighlight %}

where file.txt has following content
{% highlight sh linenos %}
file '/path/to/file/1.ts'
file '/path/to/file/2.ts'
file '/path/to/file/3.ts'
{% endhighlight %}


