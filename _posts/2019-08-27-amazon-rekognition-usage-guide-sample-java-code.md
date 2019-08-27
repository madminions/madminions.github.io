---
layout: post
published: true
title: Amazon Rekognition
image: /img/amazon-rekognition-start-page.png
---
## This Post is about usage of Amazon Rekognition Java APIs in general and Detection of unsafe contents in images in particular.

There are various open source projects also available for detection of unsafe contents in images.
Yahoo [open-nsfw](https://github.com/yahoo/open_nsfw) is one of such projects.

This post will see how to use Amazon rekognition Java API.
We will use [java sdk version 2](https://sdk.amazonaws.com/java/api/latest/)

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

This below java code supplies image as base 64 encoded byte array.

Create a java file with following code.

{% highlight java linenos %}

import java.io.File;
import java.io.FileInputStream;

import software.amazon.awssdk.core.SdkBytes;
import software.amazon.awssdk.services.rekognition.RekognitionClient;
import software.amazon.awssdk.services.rekognition.model.DetectModerationLabelsRequest;
import software.amazon.awssdk.services.rekognition.model.DetectModerationLabelsResponse;
import software.amazon.awssdk.services.rekognition.model.Image;
import software.amazon.awssdk.services.rekognition.model.ModerationLabel;

public class Test {

	private static final String accessKeyId = "XXX";
	private static final String secretKey = "XXX";
	private static final String region = "XXX";

	public static void main(String args[]) {

		try {

			System.setProperty("aws.accessKeyId", accessKeyId);
			System.setProperty("aws.secretAccessKey", secretKey);
			System.setProperty("aws.region", region);

			RekognitionClient rekognitionClient = RekognitionClient.builder().build();
			String photoPath = "path";

			DetectModerationLabelsRequest request = DetectModerationLabelsRequest
					.builder().image(Image.builder()
							.bytes(SdkBytes.fromInputStream(new FileInputStream(new File(photoPath)))).build())
					.minConfidence(60F).build();

			DetectModerationLabelsResponse result = rekognitionClient.detectModerationLabels(request);
			for (ModerationLabel label : result.moderationLabels()) {
				System.out.println("Label: " + label.name() + "\nConfidence: " + label.confidence().toString()
						+ "\nParent: " + label.parentName());

			}

		} catch (Exception e) {
			e.printStackTrace();
		}
	}

}

{% endhighlight %}

When run, We get following [moderation levels](https://docs.aws.amazon.com/rekognition/latest/dg/moderation.html) results based on image content.



One such example output might be 
~~~
Label: Violence
Confidence: 96.8038563
Parent: 
~~~

There are various limits
1. Only JPG and PNG are supported
2. If using base 64 encoded image, max file size is 5 MB
3. If using S3, max file size is 15 MB.

Here are the complete [limits](https://docs.aws.amazon.com/rekognition/latest/dg/limits.html) that you wants to look into.

I will try to cover images using s3 bucket in next post. Stay tuned.