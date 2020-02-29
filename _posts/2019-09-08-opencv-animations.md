---
layout: post
title: OpenCv Animations
published: false
---

This post is about OpenCv Animations.
So I was going through my clicked pics from one of my trip 
and thought to make a video from the images. I thought maybe
I can apply OpenCv for image animations ?

> Installation and Usage Steps

1. Install Anaconda. We can download anaconda from [here](https://www.anaconda.com/distribution/#download-section).
2. Install dependencies. Install other dependencies based on your linux distribution.
	i. conda install -c conda-forge opencv
	ii. pip install imutils
    
3. Clone this project from [github](https://github.com/madminions/videomontage).
4. Edit create_animations.py file to pass path of images to images list.
	e.g. images = ['a.jpg','b.jpg'] for a.jpg and b.jpg images in src folder.
5. run python create_animations.py. output.avi video file will be created.
6. We can use ffmpeg to add audio to our video file. For installation of ffmpeg and other commands [see](https://madminions.github.io/2019-08-18-ffmpeg-usage/).
7. Use below command to add background audio of your choice.
{% highlight python linenos %}
ffmpeg -i output.avi -i music.mp3 -c copy -shortest out.mp4
{% endhighlight %} 


Happy Coding.
