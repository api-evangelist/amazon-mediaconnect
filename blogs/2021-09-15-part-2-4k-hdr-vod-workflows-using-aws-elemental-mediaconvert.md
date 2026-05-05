---
title: "Part 2: 4K HDR VOD workflows using AWS Elemental MediaConvert and AWS Elemental Server"
url: "https://aws.amazon.com/blogs/media/part-2-4k-hdr-vod-workflows-using-aws-elemental-mediaconvert-and-aws-elemental-server/"
date: "Wed, 15 Sep 2021 16:51:40 +0000"
author: "Brian Bedard"
feed_url: "https://aws.amazon.com/blogs/media/tag/aws-elemental-mediaconnect/feed/"
---
<p><a href="https://aws.amazon.com/blogs/media/part-1-expanding-the-color-gamut-with-hdr-and-aws-elemental/"><strong>Part 1: Expanding the color gamut with HDR and </strong><strong>AWS </strong><strong>Elemental</strong></a><br /> <strong>Part 2: HDR VOD workflows using AWS Elemental Server and AWS Elemental MediaConvert (this post)</strong><br /> <a href="https://aws.amazon.com/blogs/media/part-3-live-and-vod-to-live-hdr-workflows-on-aws/"><strong>Part 3: Live and VOD-to-Live HDR workflows on AWS</strong></a></p> 
<p>You have probably heard 4K and HDR many times, but exactly what do they mean for you and your customers? What tools and services can you use today to process these video assets, and how do you avoid mistakes and get your job done efficiently? In the second part of our series, we show you how to prepare 4K HDR media assets using <a href="https://aws.amazon.com/mediaconvert/">AWS Elemental MediaConvert</a> and <a href="https://aws.amazon.com/elemental-server/">AWS Elemental Server</a>.</p> 
<p>4K UHD in general refers to a 3840×2160 video resolution, which is 4 times the HD resolution of 1920×1080. 4K is also interchangeably used as Ultra HD or UHD. The increased resolution provides improved image clarity and sharpness, and allows for larger displays. 4K DCI refers to a variation for digital cinema that is 4096 x 2160 pixels.</p> 
<p>You can use MediaConvert or AWS Elemental Server to process 4K HDR VOD assets. In addition, MediaConvert can support up to 8K, 8192×4320 resolution with HEVC encoding, 10-bit, including HDR. Customers often ask what the best video codec settings for 4K HDR content are, and how they can avoid common mistakes in media processing to increase efficiency. We put together the following as a reference to use or share with production teams for 4K HDR assets, so you don’t have to worry about the compatibly issues with MediaConvert or Elemental Server.</p> 
<p>MediaConvert and AWS Elemental Server support MXF and QuickTime container format. The following table shows detailed codecs supported by each container. Most often, MXF containers are used with VC-3 and JPEG 2000 codec, or QuickTime with ProRes codec for 4K HDR video assets.</p> 
<table> 
 <tbody> 
  <tr style="background-color: #ff9933;"> 
   <td style="padding: 5px;"><strong>Container</strong></td> 
   <td style="padding: 5px;"><strong>Video Codecs</strong></td> 
   <td style="padding: 5px;"><strong>Audio</strong></td> 
  </tr> 
  <tr style="background-color: #ffd9b3;"> 
   <td style="padding: 5px;">MXF</td> 
   <td style="padding: 5px;">Uncompressed, Apple ProRes (<a href="https://docs.aws.amazon.com/mediaconvert/latest/ug/supported-types-for-apple-prores-inputs.html">supported types</a>), AVC Intra 50/100, VC-3, DV/DVCPRO, DV25, DV50, DVCPro HD, AVC (H.264), JPEG 2000 (J2K), MPEG-2, Panasonic P2, SonyXDCam, SonyXDCam MPEG-4 Proxy</td> 
   <td style="padding: 5px;">AAC, AIFF, Dolby E frames carried in PCM streams, MPEG Audio, PCM</td> 
  </tr> 
  <tr style="background-color: #ffd9b3;"> 
   <td style="padding: 5px;">QuickTime</td> 
   <td style="padding: 5px;">Uncompressed, Apple ProRes (<a href="https://docs.aws.amazon.com/mediaconvert/latest/ug/supported-types-for-apple-prores-inputs.html">supported types</a>), AVC Intra 50/100, DivX/Xvid, DV/DVCPRO, H.261, H.262, H.263, AVC (H.264), HEVC (H.265), JPEG 2000 (J2K), MJPEG, MPEG-2, MPEG-4 part 2, QuickTime Animation (RLE)</td> 
   <td style="padding: 5px;">AAC, MP3, PCM</td> 
  </tr> 
 </tbody> 
</table> 
<p>Let’s dig into how customers can use MediaConvert and Server to prepare the content for OTT delivery.</p> 
<h2>Use MediaConvert to prepare 4K HDR for content delivery</h2> 
<p>As previously mentioned, AWS Elemental MediaConvert is a file-based video transcoding service with broadcast-grade features, and supports up to 8K, HEVC, 10-bit HDR transcoding. Based on the <a href="https://developer.apple.com/documentation/http_live_streaming/hls_authoring_specification_for_apple_devices">HLS authoring specification</a> from Apple, we touch upon the key configuration choices customers can make on the MediaConvert service.</p> 
<p>In the following example, we assume that the customer already has 4K HDR10 VOD assets existing in an <a href="https://aws.amazon.com/s3/">Amazon S3</a> bucket. For OTT delivery, we plan to create an HDR ABR stack and an SDR ABR stack, and combine them into one single master manifest. We need to ensure the following two items are in place before we start:</p> 
<ol> 
 <li>Use fragmented mp4 container instead of MPEG transport stream. This is because for HEVC video, the container format must be fMP4 per the HLS specification.</li> 
 <li>Use MediaConvert built-in color conversion feature to produce SDR content. For customers who prefer to use their own color-graded SDR VOD assets, they can use SDR video source for transcoding, and manually combine the manifests into a single master manifest.</li> 
</ol> 
<p>Now let’s take a look what are the key constructs in MediaConvert for this transcoding workflow.</p> 
<p>1. Log into <a href="https://aws.amazon.com/console/">AWS Management Console</a>, go to MediaConvert service, and <a href="https://docs.aws.amazon.com/mediaconvert/latest/ug/create-a-job.html">Create a job</a>.</p> 
<p>2. In <strong>Output groups</strong>, click <strong>Add</strong> button, select <strong>CMAF</strong> option in the popup menu.</p> 
<p>a. CMAF output group enables MediaConvert to produce fMP4 container for the output video.</p> 
<p><img alt="Add CMAF output group" class="aligncenter wp-image-9515 size-full" height="602" src="https://d2908q01vomqb2.cloudfront.net/fb644351560d8296fe6da332236b1f8d61b2828a/2021/09/13/WedSept15_20211.png" width="977" /></p> 
<p>3. Select the newly created output group, and make changes to the common parameters such as ‘<strong>Custom group name</strong>’, ‘<strong>Destination</strong>’, and provide <strong>Segment control</strong> settings and <strong>HLS specific settings</strong>.</p> 
<p>4. For segment control settings, set <strong>Fragment length</strong> to 2 seconds, and <strong>Segment length</strong> to 6 seconds. By default, <strong>Write HLS manifest</strong> and <strong>Write Dash manifest</strong> are both checked.</p> 
<p><img alt="Settings for segment control, fragment length, segment length." class="aligncenter wp-image-9516 size-full" height="179" src="https://d2908q01vomqb2.cloudfront.net/fb644351560d8296fe6da332236b1f8d61b2828a/2021/09/13/WedSept15_20212.png" width="977" /></p> 
<p>5. For <strong>HLS specific settings</strong>, set <strong>Manifest duration format</strong> to Floating point, set <strong>Include resolution</strong> to Include, and uncheck <strong>Client cache</strong> checkbox. Set <strong>Codec specification</strong> to RFC 6381.</p> 
<p><img alt="" class="size-full wp-image-9517 aligncenter" height="252" src="https://d2908q01vomqb2.cloudfront.net/fb644351560d8296fe6da332236b1f8d61b2828a/2021/09/13/WedSept15_20213.png" width="977" /></p> 
<p>6. The next step is to add multiple outputs. We will create a total of 8 video outputs, and 2 audio outputs. The following table shows the output details.</p> 
<p><strong>HDR Outputs:</strong></p> 
<ul> 
 <li>1280x720p, H.264 (AVC): QVBR, QVBR quality level 8, Multi pass HQ, 3.5Mbps</li> 
 <li>1920x1080p, H264 (AVC): QVBR, QVBR quality level 8, Multi pass HQ, 6Mbps</li> 
 <li>1920x1080p, H265 (HEVC): QVBR, QVBR quality level 8, Multi pass HQ, 5.5Mbps</li> 
 <li>3840x2160p, H265 (HEVC): QVBR, QVBR quality level 9, Multi pass HQ, 15Mbps</li> 
</ul> 
<p>7. Now it’s time to decide if you are targeting HDR10 or HDR10+, which supports dynamic metadata. We walk through both in steps 8 and 9, but you should only choose one or the other.</p> 
<p>8.<strong> HDR10 →</strong> For all HDR outputs, make sure set Profile to <strong>High 10-bit </strong>for H.264 codec, and set Profile to <strong>Main10/Main </strong>for H.265 codec. For all HDR output, enable Color corrector in the Preprocessors section, and set Color space conversion to <strong>Force HDR 10</strong>.</p> 
<p><img alt="Color corrector settings" class="aligncenter wp-image-9518 size-full" height="241" src="https://d2908q01vomqb2.cloudfront.net/fb644351560d8296fe6da332236b1f8d61b2828a/2021/09/13/WedSept15_20214.png" width="977" /></p> 
<p>9.<strong> HDR10+ →</strong> enable HDR10+ in the Preprocessors section, and set the <strong>Mastering Monitor Nits</strong> to match the settings for the monitor where the source was graded and <strong>Target Monitor Nits</strong> to the expected peak target monitor, which is usually <strong>1000</strong>.</p> 
<p><img alt="Enable HDR10+ settings" class="aligncenter wp-image-9519 size-full" height="172" src="https://d2908q01vomqb2.cloudfront.net/fb644351560d8296fe6da332236b1f8d61b2828a/2021/09/13/WedSept15_20215.png" width="977" /></p> 
<p>10. For all SDR outputs, we use built-in color conversion feature to convert HDR to SDR color space. Enable Color corrector in the Preprocessors section, and set Color space conversion to <strong>Force Rec. 709</strong>.</p> 
<p><strong>SDR Outputs:</strong></p> 
<ul> 
 <li>1280x720p, H.264 (AVC): QVBR, QVBR quality level 8, Multi pass HQ, 3Mbps</li> 
 <li>1920x1080p, H264 (AVC): QVBR, QVBR quality level 8, Multi pass HQ, 5Mbps</li> 
 <li>1920x1080p, H265 (HEVC): QVBR, QVBR quality level 8, Multi pass HQ, 5Mbps</li> 
 <li>3840x2160p, H265 (HEVC): QVBR, QVBR quality level 9, Multi pass HQ, 12Mbps</li> 
</ul> 
<p><strong>Audio Outputs:</strong></p> 
<ul> 
 <li>AAC audio 96kbps, Stereo, 48Khz</li> 
 <li>AAC audio 256kbps, Stereo, 48Khz</li> 
</ul> 
<p>11. Now you have setup the complete MediaConvert codec settings, finish the rest configuration, and you can start submit your transcoding job.</p> 
<h2>Use AWS Elemental Server to prepare 4K, HDR content delivery</h2> 
<p>Customers who have AWS Elemental Server appliances can use it to prepare 4K HDR assets for OTT delivery.</p> 
<p>1. To create a transcoding job in AWS Elemental Server web user interface, in the <strong>Job Queue</strong> section, click <strong>New Job</strong> button.</p> 
<p>2. Use <strong>CMAF</strong> output group to generate fragmented mp4 container format. Set <strong>Segment Length</strong> to 6 seconds.</p> 
<p><img alt="Elemental Server CMAF Output" class="aligncenter wp-image-9521 size-full" height="427" src="https://d2908q01vomqb2.cloudfront.net/fb644351560d8296fe6da332236b1f8d61b2828a/2021/09/13/WedSept15_20216.png" width="977" /></p> 
<p>3. Expand <strong>Advanced</strong> option, find <strong>HLS Specific Settings</strong>, check <strong>HLS Use EFC 6381 (Pantos 7) CODEC identifier</strong>s checkbox.</p> 
<p><img alt="HLS Specific Settings on Elemental Server" class="aligncenter wp-image-9522 size-full" height="187" src="https://d2908q01vomqb2.cloudfront.net/fb644351560d8296fe6da332236b1f8d61b2828a/2021/09/13/WedSept15_20217.png" width="977" /></p> 
<p>4. In <strong>New Output</strong> section, click <strong>Add Output</strong> button to add an output, a new <strong>Stream</strong> is automatically created to associate with the new output. In the <strong>Streams</strong> section, for each stream, remove <strong>Audio</strong> from video only streams, and remove <strong>Video</strong> from audio only streams.</p> 
<p>5. Configure your video and audio using the parameters in the sample configurations section. For streams using HEVC codec, the <strong>Write HVC1 for H.265</strong> checkbox shows in <strong>Outputs</strong> section, make sure this checkbox is checked to conform to the section 1.10 in HLS authoring specification.</p> 
<p><code>1.10. You SHOULD use video formats in which the parameter sets are stored in the sample descriptions, rather than the samples. (that is, use 'avc1', 'hvc1', or 'dvh1' rather than 'avc3', 'hev1', or 'dvhe'.)</code></p> 
<h2>Sample Configurations</h2> 
<p>The following is an example of the Elemental Server UI. You can provide a name modifier to each of the output, and it is used as part of the output variant name in the manifest file.</p> 
<p><img alt="" class="size-full wp-image-9523 aligncenter" height="318" src="https://d2908q01vomqb2.cloudfront.net/fb644351560d8296fe6da332236b1f8d61b2828a/2021/09/13/WedSept15_20219.png" width="977" /></p> 
<p>The following is a sample output profile you can use as a reference.</p> 
<p><strong>Video Stream HDR Outputs<br /> </strong><br /> 1280x720p, H.264 (AVC): <strong>Rate Control Mode: </strong>QVBR 2-pass, <strong>Max Bitrate:</strong> 3.5Mbps, <strong>Quality Level</strong>: 8,<strong> GOP</strong>: 2.0, <strong>Units</strong>: Seconds, <strong>B Frames</strong>: 2, <strong>Closed GOP Cadence</strong>: 1, <strong>Reference Frames</strong>: 3, <strong>GOP Reference B-Frame</strong>: check, <strong>Dynamic Sub-Gop</strong>: check, <strong>Scene Change Detect</strong>: Transition detection, <strong>Profile</strong>: High 10-bit, <strong>Density vs Quality</strong>: 0</p> 
<p>1920x1080p, H264 (AVC): <strong>Rate Control Mode: </strong>QVBR 2-pass, <strong>Max Bitrate:</strong> 6.0Mbps, <strong>Quality Level</strong>: 8. Follow 720p profile for the rest parameter settings.</p> 
<p>1920x1080p, H265 (HEVC): <strong>Rate Control Mode: </strong>QVBR 2-pass, <strong>Max Bitrate:</strong> 5.5Mbps, <strong>Quality Level</strong>: 8, <strong>GOP</strong>: 2.0, <strong>Units</strong>: Seconds, <strong>B Frames</strong>: 2, <strong>Closed GOP Cadence</strong>: 1, <strong>Reference Frames</strong>: 3, <strong>GOP Reference B-Frame</strong>: check, <strong>Dynamic Sub-Gop</strong>: check, <strong>Scene Change Detect</strong>: Transition detection, <strong>Profile</strong>: Main10/Main, <strong>Density vs Quality</strong>: 0.5</p> 
<p>3840x2160p, H265 (HEVC): <strong>Rate Control Mode: </strong>QVBR 2-pass, <strong>Max Bitrate:</strong> 15Mbps, <strong>Quality Level</strong>: 9, <strong>GOP</strong>: 2.0, <strong>Units</strong>: Seconds, <strong>B Frames</strong>: 2, <strong>Closed GOP Cadence</strong>: 1, <strong>Reference Frames</strong>: 1, <strong>GOP Reference B-Frame</strong>: uncheck, <strong>Dynamic Sub-Gop</strong>: uncheck, <strong>Scene Change Detect</strong>:Enabled, <strong>Profile</strong>: Main10/Main, <strong>Density vs Quality</strong>: 0.5</p> 
<p>For all HDR output, enable <strong>Color corrector</strong> in the <strong>Preprocessors</strong> section, and set <strong>Color space conversion</strong> to Force HDR 10.</p> 
<p><img alt="" class="size-full wp-image-9524 aligncenter" height="139" src="https://d2908q01vomqb2.cloudfront.net/fb644351560d8296fe6da332236b1f8d61b2828a/2021/09/13/WedSept15_202110.png" width="977" /></p> 
<p><strong>Video Stream SDR Outputs</strong></p> 
<ul> 
 <li>1280x720p, H.264 (AVC): <strong>Rate Control Mode: </strong>QVBR 2-pass, <strong>Max Bitrate:</strong> 3.0Mbps, <strong>Quality Level</strong>: 8,<strong> GOP</strong>: 2.0, <strong>Units</strong>: Seconds, <strong>B Frames</strong>: 2, <strong>Closed GOP Cadence</strong>: 1, <strong>Reference Frames</strong>: 3, <strong>GOP Reference B-Frame</strong>: check, <strong>Dynamic Sub-Gop</strong>: check, <strong>Scene Change Detect</strong>: Transition detection, <strong>Profile</strong>: Main, <strong>Density vs Quality</strong>: 0</li> 
 <li>1920x1080p, H264 (AVC): <strong>Rate Control Mode: </strong>QVBR 2-pass, <strong>Max Bitrate:</strong> 5.0Mbps, <strong>Quality Level</strong>: 8,<strong> GOP</strong>: 2.0, <strong>Units</strong>: Seconds, <strong>B Frames</strong>: 2, <strong>Closed GOP Cadence</strong>: 1, <strong>Reference Frames</strong>: 3, <strong>GOP Reference B-Frame</strong>: check, <strong>Dynamic Sub-Gop</strong>: check, <strong>Scene Change Detect</strong>: Transition detection, <strong>Profile</strong>: High, <strong>Density vs Quality</strong>: 0</li> 
 <li>1920x1080p, H265 (HEVC): <strong>Rate Control Mode: </strong>QVBR 2-pass, <strong>Max Bitrate:</strong> 4.5Mbps, <strong>Quality Level</strong>: 8,<strong> GOP: 2.0, Units: Seconds, B Frames: </strong>2<strong>, Closed GOP Cadence: </strong>1<strong>, Reference Frames: </strong>3<strong>, GOP Reference B-Frame: </strong>check<strong>, Dynamic Sub-Gop: </strong>check<strong>, Scene Change Detect: </strong>Transition detection<strong>, Profile: </strong>Main/High<strong>, Density vs Quality: </strong>0.5</li> 
 <li>3840x2160p, H265 (HEVC): <strong>Rate Control Mode: </strong>QVBR 2-pass, <strong>Max Bitrate:</strong> 12Mbps, <strong>Quality Level</strong>: 8,<strong> GOP: 2.0, Units: Seconds, B Frames: </strong>2<strong>, Closed GOP Cadence: </strong>1<strong>, Reference Frames: 1, GOP Reference B-Frame: </strong>uncheck<strong>, Dynamic Sub-Gop: un</strong>check<strong>, Scene Change Detect: </strong>Enabled<strong>, Profile: </strong>Main/High<strong>, Density vs Quality: </strong>0.5</li> 
</ul> 
<p>For all SDR output, enable <strong>Color corrector</strong> in the <strong>Preprocessors</strong> section, and set <strong>Color space conversion</strong> to Force Rec. 709.</p> 
<p><img alt="" class="size-full wp-image-9525 aligncenter" height="139" src="https://d2908q01vomqb2.cloudfront.net/fb644351560d8296fe6da332236b1f8d61b2828a/2021/09/13/WedSept115_202112.png" width="977" /></p> 
<p><strong>Audio Stream Outputs</strong></p> 
<ul> 
 <li>AAC audio 96kbps, Stereo, 48Khz</li> 
 <li>AAC audio 256kbps, Stereo, 48Khz</li> 
</ul> 
<h2>Summary</h2> 
<p>In this post, we showed you how to use AWS Elemental MediaConvert and AWS Elemental Server to prepare a 4K HDR video asset for OTT delivery, and how to put them into an ABR stack. We provided detailed configuration steps, and sample configuration options to help you navigate through the options quickly, and gave you flexibility to adjust them to your own needs. In the third post of this series, we dive into live HDR and VOD playout workflows.</p> 
<p><strong>Reference:</strong></p> 
<p>HLS Authoring Specification for Apple Devices: <a href="https://developer.apple.com/documentation/http_live_streaming/hls_authoring_specification_for_apple_devices">https://developer.apple.com/documentation/http_live_streaming/hls_authoring_specification_for_apple_devices</a></p>
