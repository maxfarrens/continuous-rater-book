# Serving video stimuli


## The basics

`continuous-rater` relies on served audio/video stimuli. You can use a local server for your stimuli, or rely on web services provided by companies like Amazon and Google. **I recommend against using Google Firebase to store and serve stimuli due to slow load times and issues with latency** but the app is compatible with this option. 

Whether you use a local server or a cloud-based option, there are several important steps that need to be taken to ensure optimal results:

1. **Set up a Content Delivery Network (CDN)** that will cache your stimuli content at the network edge and ensure fast and reliable delivery of audio/video. A CDN can (and should) be used with a cloud server or a local server and leads to significantly decreased buffer and load times. To learn more about CDNs in general, click [here](https://www.cloudflare.com/learning/cdn/what-is-a-cdn/).<p>&nbsp;</p>

2. **Name your files without special characters or spaces** to ensure compatibility with `continuous-rater`. For example, if a video stimulus is called **Cops Don't Cry** and is in the `.mp4` format, name the file **CopsDontCry.mp4**, removing all special characters and spaces. These exact names are used in the stimuli table your Firebase database, so simpler is better. 


## Testing with local files

When testing the app locally, it can be helpful to use stimuli directly available on your computer rather than dealing with storage, serving and CDNs. However, there are security issues associated with this, and so you still need to "serve" your local stimuli to use them with the app. To accomplish this, you can use a web server that runs offline locally (like [this](https://chrome.google.com/webstore/detail/web-server-for-chrome/ofhbbkphhbklhfoeikjpcbhemlocgigb/related) one!), and upload a local folder of your stimuli to that server. You can then access each stimulus through that server. When you create your stimuli table in Firebase (see Building a stimuli table in Firebase), fill in the local server URL to each video stimulus as the Value.


## Using AWS for serving

When it comes time to build the app for real data collection, you will need to serve your stimuli. I highly recommend against serving stimuli on a local server due to issues surrounding reliability/scalability (but the app will still function if you choose to do so). Instead, I recommend using **[AWS S3](https://aws.amazon.com/s3/) to store videos and [AWS CloudFront](https://aws.amazon.com/cloudfront/) (CDN) to deliver content to end-users**. 

First, create an S3 storage bucket, and upload all video stimuli into that bucket. Then, create a CloudFront distribution that points to that S3 bucket. Read the docs ([S3](https://docs.aws.amazon.com/AmazonS3/latest/gsg/GetStartedWithS3.html) and [CloudFront](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/Introduction.html)) to get started. **Click [here](https://aws.amazon.com/cloudfront/streaming/) for a very helpful video-streaming tutorial** using this AWS stack. 

If you use this option, you can also easily integrate [AWS MediaConvert](https://aws.amazon.com/mediaconvert/) to control file size, resolution and many other parameters related to your video stimuli. 


## Quick tips


1. **Lower resolution = faster streaming**. I recommend converting all videos to 480p and .mp4 format for simple and reliable streaming (unless resolution is important to your study). You can either do this locally, or use [AWS MediaConvert](https://aws.amazon.com/mediaconvert/) to convert videos already uploaded to S3 storage (but then make sure your CDN points to these converted videos, not the originals!). `continuous-rater` is not currently compatible with the HLS protocol.<p>&nbsp;</p>

2. **Check your files permissions**. If using AWS S3 (or any other cloud storage solution) ensure that the storage bucket containing your media (as well as the media itself) allows the necessary read access permissions, otherwise your CDN will not be able to serve your stimuli properly, and `continuous-rater` will not be able to display your videos.<p>&nbsp;</p>

3. **Validate streaming functionality early**. You can check to ensure your streaming is working by appending a stimulus filename to the host or CDN url. Use this url to watch the clip directly in your browser. <p>&nbsp;</p>

	For example, if my CDN is [https://dfhda873u.cloudfront.net/]() and my video is CopsDontCry.mp4, then I can test my distribution chain using [https://dfhda873u.cloudfront.net/CopsDontCry.mp4](). Once validated, add your stimuli to the stimuli table in Firebase by providing name/url pairings (e.g. CopsDontCry in the Field category and [https://dfhda873u.cloudfront.net/CopsDontCry.mp4]() in the Value category). 




	

