---
layout: post
title: OpenCv Animations
published: true
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
	We are using zoom in effect for each image.
6. We can use ffmpeg to add audio to our video file. For installation of ffmpeg and other commands [see](https://madminions.github.io/2019-08-18-ffmpeg-usage/).
7. Use below command to add background audio of your choice.
{% highlight python linenos %}
ffmpeg -i output.avi -i music.mp3 -c copy -shortest out.mp4
{% endhighlight %} 


##

Code Explaination

All code is written in Animation.py file.

This below method gets called. it returns images in a list and we stitches
all images together to create a video. We paste all zoom in images we get
on top of blur image and create a sequence of it.

{% highlight python linenos %}
def img_animation_zoom_in(self, orig_img, blur, fr=30):
        big_img_size = blur.shape
        ret_img = []

        img_list = self.zoom_in_until(blur, orig_img.copy())

        for iimg, img_resz in enumerate(img_list):
            sml_img_size = img_resz.shape
            y_offset = int((big_img_size[1] - sml_img_size[1]) / 2)
            x_offset = int((big_img_size[0] - sml_img_size[0]) / 2)
            im = Image.fromarray(img_resz).convert('RGBA')
            blur_im = Image.fromarray(blur).convert('RGBA')

            # paste rotated image on blur background
            blur_im.paste(im, (y_offset, x_offset), im)
            ret = np.array(blur_im.convert('RGB'))
            ret_img.append(ret)
        return ret_img
{% endhighlight %} 


We get zoomed images list from this method. Here we are zooming from 0.7 times
original image to become 0.9 time of original image. You can tweak this values
for more animations.

{% highlight python linenos %}
def zoom_in_until(self, img1, img2):
        """
        zoom out on img2 until img2 becomes less than scale times of img1
        :param img1: blur background image
        :param img2: to be resized image
        :return:
        """
        scale = 0.9
        ret_img = []
        orig_img = img2.copy()
        ((h1, w1), (h2, w2)) = (img1.shape[:2], img2.shape[:2])
        w2f = int(w1 * scale)
        h2f = int(h1 * scale)

        # rescale orig img to scale times to zoom in
        h2 = int(h1 * 0.7)
        w2 = int(w1 * 0.7)
        img2 = imutils.resize(image=orig_img, height=h2, inter=Constants.INTERPOLATION)
        ret_img.append(img2.copy())

        while w2 < w2f and h2 < h2f:
            h2old = h2
            h2 = int(h2 * 1.0018)
            if h2old == h2:
                h2 += 2
            img2 = imutils.resize(image=orig_img, height=h2, inter=Constants.INTERPOLATION)
            ret_img.append(img2.copy())

        return ret_img
{% endhighlight %} 


Similarly we can create animations for zoom out, coming from left/right side, from transparent
to blur using methods written in Animations.py file.

Please feel free to contribute for more Animations.

Happy Coding.
