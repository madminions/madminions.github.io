---
layout: post
published: false
title: Amazon-Rekognition-Usage-Guide.md
---
## This Post is about usage of Amazon Rekognition Java APIs in general and Detection of unsafe contents in images in particular.

There are various open source projects also available for detection of unsafe contents in images.
Yahoo [open-nsfw](https://github.com/yahoo/open_nsfw) is one of such projects.

This post will see how to use Amazon rekognition Java API.
We will use java sdk version 2. [link](https://sdk.amazonaws.com/java/api/latest/)

Create a new Maven project with pom file having following dependency.
{% highlight xml linenos %}
<dependency>
  <groupId>software.amazon.awssdk</groupdId>
  <artifactId>rekognition</artifactId>
  <version>2.7.27</version>
</dependency>
{% endhighlight %}


There are two methods for using images
1. Supplying Image as base 64 encoded byte array
2. Uploading in S3 bucket. User should have sufficient access to image.

Create a java file with following code.

When run, We get following moderation results based on image content

[moderation levels](https://docs.aws.amazon.com/rekognition/latest/dg/moderation.html)

There are various limits
1. Only JPG and PNG are supported
2. If using base 64 encoded image, max file size is 5 MB
3. If using S3, max file size is 15 MB.

[limits](https://docs.aws.amazon.com/rekognition/latest/dg/limits.html)

