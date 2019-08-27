---
layout: post
published: false
title: Amazon-Rekognition-Usage-Guide.md
---
## This Post is about usage of Amazon Rekognition APIs in general and Detection of unsafe contents in images in particular.

There are various open source projects also available for detection of unsafe contents in images.
Yahoo [open-nsfw](https://github.com/yahoo/open_nsfw) is one of such projects.

This post will see how to use Amazon rekognition API.
We will use java sdk version 2. [link](https://sdk.amazonaws.com/java/api/latest/)

Create a new Maven project with pom file having following dependency.
{% highlight xml linenos %}
<dependency>
  <groupId>software.amazon.awssdk</groupdId>
  <artifactId>rekognition</artifactId>
  <version>2.7.27</version>
</dependency>
{% endhighlight %} 


