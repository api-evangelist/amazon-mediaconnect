---
title: "Live media workflows on AWS: To compress, or to not compress?"
url: "https://aws.amazon.com/blogs/media/metfc-live-media-workflows-on-aws-to-compress-or-to-not-compress/"
date: "Mon, 23 Aug 2021 20:47:54 +0000"
author: "Noor Hassan"
feed_url: "https://aws.amazon.com/blogs/media/tag/aws-elemental-mediaconnect/feed/"
---
<h2>Introduction</h2> 
<p>In a cloud migration journey, there are many decisions that need to be made. One of the primary decisions for live media workflows is how to bring content from on-premises to the cloud. For example, how to take an event that is being shot at a sporting arena to the cloud for processing, and then — if needed — from the cloud to on-premises for distribution over satellite. In this post, we discuss two possible paths for a live workflow: compressed and uncompressed. We identify the various considerations a broadcaster can use to make informed decisions based on what is best suited for the workflow.</p> 
<h2>End-to-end live media chain</h2> 
<p>To start, let’s dive into the main components of an end-to-end live media workflow, from content production to distribution:</p> 
<p>When content is transmitted from on-premises such as venues, studios, and outside broadcast production in cloud, this is considered a contribution signal workflow, or the “first mile” of the content. Preserving the highest picture quality for the contribution is important to mitigate the impact of encoding-decoding cycles and since processing of production workflows typically introduces latencies that are cascaded further into the distribution, ensuring minimal latency is also key for contribution workflows.</p> 
<p>Distribution signal flow, or the “last mile” of the content is where it is transported to a variety of delivery platforms. In distribution, bandwidth is a key consideration, where bitrates are lower in comparison to the “first mile” journey. For example, satellite delivery has fixed uplink bandwidth calling for use of statistical multiplexing, where variable bitrate (VBR) is used to assign more bits to complex content such as a sporting clip compared to less complex content such as a talking head in a news room. Another example is over-the-top (OTT) applications, where adaptive bitrate (ABR) transcoding is used for delivery over the internet for multiple devices using variable bitrates and resolutions. Quality is also an important a consideration for distribution workflows as there can be additional processing before the final leg for delivery to the consumer such as cable providers, Multiple Video Programming Distributor (MVPD).</p> 
<div class="wp-caption aligncenter" id="attachment_9282" style="width: 757px;">
 <img alt="Figure 1: End-to-end live workflow example" class="wp-image-9282 size-full" height="379" src="https://d2908q01vomqb2.cloudfront.net/fb644351560d8296fe6da332236b1f8d61b2828a/2021/08/23/August23_20211.png" width="747" />
 <p class="wp-caption-text" id="caption-attachment-9282"><em>Figure 1: End-to-end live workflow example</em></p>
</div> 
<p>Now let’s examine some of the key considerations to account for when choosing a method:</p> 
<ol> 
 <li>The type of media content you plan to capture, process, and deliver.</li> 
 <li>The desired reliability with added bandwidth overhead either through re-transmission, failover, and error correction mechanisms.</li> 
 <li>Acceptable latency from the time content is captured to when it’s delivered on a viewer’s device (aka glass-to-glass) is usually in seconds, whereas latency to an operator’s interface is typically required to be in milliseconds. The size of packets, state of the network, and buffering because of re-transmission or error correction overhead affect these latency requirements.</li> 
 <li>Interoperability with the current or future roadmap for on-premises and/or AWS Cloud environments.</li> 
</ol> 
<p>Now, that we have an understanding of the workflow, let’s look deeper into each of the path – compressed and uncompressed</p> 
<h2>The compressed method</h2> 
<p>The fundamental concept behind a compressed path is to leverage compression algorithms and codecs for audio and video is to reduce the size of the asset while preserving the details that the human eye can see (or is sensitive to).</p> 
<p>Some of those major codecs include H.264 (AVC), H.265 (HEVC), and JPEG (2000 and XS).<br /> Compression algorithms can be divided into lossy and lossless, however, some codecs support a combination of both.</p> 
<p>Compression considerations:</p> 
<p>Compressed workflows consider aspects such as the required resolution (for example supporting UHD 4K signals), bit depth / color sub-sampling (for example supporting 4:2:2 10 Bit), and HDR requirements if any. Additionally, understanding the latency requirements for the application and the available bandwidth is key. Lastly, an existing or future ecosystem choice influences codec(s) for the workflow.</p> 
<p>H.264 and H.265 (HEVC):</p> 
<p>MPEG-4 AVC Part 10 (also known as H.264), one of the most commonly used compression codecs throughout the industry today, and its successor High Efficiency Video Codec (HEVC) or H.265 are codec examples of lossy compression standards, with latencies ranging from milliseconds to seconds.</p> 
<p>H.264 and HEVC codecs use the concepts of I (Intra or Key frame), P (Predictive Frame), B (Bi-directional frame) frames in addition to Group of Pictures (GOP).<br /> H.264 (and HEVC) uses intra-frame in addition to inter-frame compression.</p> 
<p>To illustrate the difference between H.264 and HEVC, consider the simple image in Figure 2 of an elephant, green grass, and blue sky. Instead of compressing every pixel, H.264 intra-frame compression breaks down the image into blocks, since the color is not really changing, the algorithm remembers the blocks instead, reducing the overall file size. With inter-frame, think of multiple images (for example if the elephant is moving). Some information across the images is repeated – such as color and position of the grass or the blue sky, and so the algorithm uses the same information it captured from the first frame, and then looks for the differences across the frames.</p> 
<div class="wp-caption aligncenter" id="attachment_9283" style="width: 166px;">
 <img alt="Figure 2: Simple image example" class="wp-image-9283 size-full" height="175" src="https://d2908q01vomqb2.cloudfront.net/fb644351560d8296fe6da332236b1f8d61b2828a/2021/08/23/August23_20212.png" width="156" />
 <p class="wp-caption-text" id="caption-attachment-9283"><em>Figure 2: Simple image example</em></p>
</div> 
<p>HEVC works in a similar fashion, but supports dynamic and larger block sizes than H.264 (up to 64×64 compared to 16×16 for H.264), therefore reducing the number of blocks the algorithm needs to remember, which reduces the overall file size further. Additionally, HEVC provides approximately 50% more efficiency compared to H.264, this means saving approximately 50% of the bitrate at the same quality of coding. Exact bitrate savings vary depending on video resolutions, color bit depth…etc.</p> 
<p>H.264 and HEVC are used heavily in distribution applications such as OTT distribution, satellite, and over-the-air (OTA), but can also be used for contribution while leveraging higher bitrates, bit depths and color sampling.</p> 
<p>JPEG2000:</p> 
<p>JPEG2000 is a widely adopted intra-frame compression codec that is used for contribution workflows that require low latency, such as remote production, since it supports low end-to-end latency (less than one frame is possible) using independent JPEG2000 stripes (stripes are one or more rows of pixels). Similar to HEVC, the standard accommodates both lossless and lossy compression techniques depending on the compression ratio used (such as 2:1, 16:1, or 30:1 in some applications).</p> 
<p>Some of the recent additions to this codec are:</p> 
<ol> 
 <li>VSF TR01 recommendations that include support for UHD 4k, 4:4:4:4 chroma sub-sampling, and higher resolutions through the addition of “block mode”.</li> 
 <li>High throughput JPEG 2000 as a new addition to the JPEG2000 suite, which enables faster coding and lower complexity.</li> 
</ol> 
<p>JPEG XS:</p> 
<p>JPEG XS is a new intra-frame, lightweight compression standard with compression ratios ranging from 2:1 to 10:1, providing a very high picture quality (also known as a subjective term ‘visually lossless’). In comparison with JPEG 2000, JPEG XS supports much lower latency – down to a fraction of a frame or a couple of frame lines. This codec is ideal for CPU and GPU environments, has lower complexity, and is a faster compression compared to JPEG 2000.</p> 
<p>JPEG XS can be transported over MPEG 2 TS, or part of 2110-22 stack, which makes it favorable for many broadcasters who have adopted ST2110 on-premises. JPEG XS as part of ST 2110-22 stack (where – 22 is the compressed video essence of the ST 2110 standard), helps with bandwidth savings for production workflows at high resolution (4K, 8K, and 16K) in comparison to uncompressed ST 2110-20 flows. Additionally, JPEG XS can tolerate multiple encode-decode processes without quality degradation.</p> 
<h2>The uncompressed method</h2> 
<p>Workloads that require high-performance connectivity and uncompressed live video transport have been historically deployed on-premises, so customers were unable to take advantage of the various benefits of the cloud like agility and scalability. <a href="https://aws.amazon.com/media-services/resources/cdi/">AWS Cloud Digital Interface</a> (AWS CDI) was created to unlock capability for high bandwidth uncompressed live video workflows in AWS without sacrificing any latency, reliability, content, and interoperability.</p> 
<p>CDI is optimized for live video applications in the cloud, and can support latencies equivalent to SDI and 2110 for resolutions up to 2160p 60 frames per second. It removes the complexity of encoding/decoding per application, adds flexibility of independent transport of video, audio, and ancillary metadata. CDI uses <a href="https://ieeexplore.ieee.org/document/9167399">AWS Scalable Reliable Datagram</a> (AWS SRD) to allow transport of high throughput with low latency reliably. AWS SRD is designed for the <a href="https://aws.amazon.com/ec2/?ec2-whats-new.sort-by=item.additionalFields.postDateTime&amp;ec2-whats-new.sort-order=desc">Amazon Elastic Compute Cloud</a> (Amazon EC2) networks within an <a href="https://aws.amazon.com/about-aws/global-infrastructure/regions_az/">Availability Zone</a> (AZ) and when combined with 100Gbps <a href="https://aws.amazon.com/hpc/efa/">Elastic Fabric Adapter</a> (EFA), provides a kernel bypass method to transport payloads. The AWS CDI software development kit (SDK) is available as open source to provide independent software vendors (ISVs) the ability to build low latency, scalable, reliable cloud-based video applications.</p> 
<h2>Choosing the right tools for the job</h2> 
<p>When choosing a method, all criteria(s) are to be considered for the journey of the signal – for example the type of content, reliability, acceptable latency, or interoperability as shown in Figure 3.</p> 
<div class="wp-caption aligncenter" id="attachment_9284" style="width: 951px;">
 <img alt="Figure 3: Matrix for consideration criteria" class="wp-image-9284 size-full" height="445" src="https://d2908q01vomqb2.cloudfront.net/fb644351560d8296fe6da332236b1f8d61b2828a/2021/08/23/August23_20213.png" width="941" />
 <p class="wp-caption-text" id="caption-attachment-9284"><em>Figure 3: Matrix for consideration criteria</em></p>
</div> 
<p>&nbsp;</p> 
<p>To help simplify various considerations and their impact, the following tables outline workflows, recommended tools for the job, and latency ranges:</p> 
<p>&nbsp;</p> 
<table> 
 <tbody> 
  <tr style="background-color: #ff9933;"> 
   <td></td> 
   <td style="padding: 5px;"><strong>On-premises first mile contribution to AWS Cloud</strong></td> 
   <td style="padding: 5px;"><strong>Live workflow in AWS Cloud</strong></td> 
   <td style="padding: 5px;"><strong>Distribution in AWS Cloud</strong></td> 
   <td style="padding: 5px;"><strong>Last mile distribution on-premises </strong></td> 
  </tr> 
  <tr style="background-color: #ffd9b3;"> 
   <td style="padding: 5px;"><strong>Method</strong></td> 
   <td style="padding: 5px;">Compressed</td> 
   <td style="padding: 5px;">Compressed / Uncompressed</td> 
   <td style="padding: 5px;">Compressed</td> 
   <td style="padding: 5px;">Compressed</td> 
  </tr> 
  <tr style="background-color: #fff2e6;"> 
   <td style="padding: 5px;"><strong>AWS Tool</strong></td> 
   <td style="padding: 5px;"><a href="https://aws.amazon.com/elemental-live/">AWS Elemental Live</a> (multi-codec support)</td> 
   <td style="padding: 5px;">AWS Media Services (<a href="https://aws.amazon.com/mediaconnect/">AWS Elemental MediaConnect</a>, <a href="https://aws.amazon.com/medialive/">AWS Elemental MediaLive</a>, <a href="https://aws.amazon.com/mediapackage/">AWS Elemental MediaPackage</a>)</td> 
   <td style="padding: 5px;">AWS Media Services (<a href="https://aws.amazon.com/mediaconnect/">AWS Elemental MediaConnect</a>, <a href="https://aws.amazon.com/medialive/">AWS Elemental MediaLive</a>, <a href="https://aws.amazon.com/mediapackage/">AWS Elemental MediaPackage</a>, <a href="https://aws.amazon.com/ivs/">Amazon Interactive Video Service</a>)</td> 
   <td style="padding: 5px;">AWS Elemental Live (multi-codec support)</td> 
  </tr> 
  <tr style="background-color: #ffd9b3;"> 
   <td style="padding: 5px;"><strong>Format / Codec</strong></td> 
   <td style="padding: 5px;">H.264, HEVC, JPEG XS</td> 
   <td style="padding: 5px;">H.264, HEVC, JPEG XS, CDI</td> 
   <td style="padding: 5px;">H.264, HEVC, JPEG XS</td> 
   <td style="padding: 5px;">H.264, HEVC</td> 
  </tr> 
 </tbody> 
</table> 
<p>Latency ranges*:</p> 
<table> 
 <tbody> 
  <tr style="background-color: #ff9933;"> 
   <td style="padding: 5px;"></td> 
   <td style="padding: 5px;"><strong>Latency Range</strong></td> 
  </tr> 
  <tr style="background-color: #ffd9b3;"> 
   <td style="padding: 5px;"><strong>Uncompressed (CDI)</strong></td> 
   <td style="padding: 5px;">&lt; 1 Frame</td> 
  </tr> 
  <tr style="background-color: #fff2e6;"> 
   <td style="padding: 5px;"><strong>JPEG XS</strong></td> 
   <td style="padding: 5px;">~ 1 Frame</td> 
  </tr> 
  <tr style="background-color: #ffd9b3;"> 
   <td style="padding: 5px;"><strong>HEVC</strong></td> 
   <td style="padding: 5px;">250ms – 2 seconds</td> 
  </tr> 
 </tbody> 
</table> 
<p>*The latency ranges provided aim to give readers a high-level idea. Latencies vary depending on different conditions such as network state, operation profiles and configurations, and workflow requirements (for example: output formats and streaming protocols)</p> 
<p>Let’s consider the following example production workflow. In order to move signals from an on-premises facility to the cloud, JPEG XS can be used to keep latency at a minimum while transporting over diverse network paths. AWS Elemental MediaConnect can be leveraged to ingest those signals, as it supports SMPTE 2022-7 for seamless failover, and output CDI within the AZ. Within each AZ, live production and post production workflows can leverage uncompressed CDI. For example, live production with playout, production switchers, audio mixers, replay servers, ingest, and closed captioning. Once processed, the live channels can then be compressed using AWS Elemental MediaLive distribution workflows such as satellite uplink and OTT.</p> 
<p>The following is an example architecture, which can be modified based on your use case/application.</p> 
<div class="wp-caption aligncenter" id="attachment_9285" style="width: 953px;">
 <img alt="Figure 4: Example architecture of live production work flow in AWS using CDI" class="wp-image-9285 size-full" height="460" src="https://d2908q01vomqb2.cloudfront.net/fb644351560d8296fe6da332236b1f8d61b2828a/2021/08/23/August23_20214.png" width="943" />
 <p class="wp-caption-text" id="caption-attachment-9285"><em>Figure 4: Example architecture of live production work flow in AWS using CDI</em></p>
</div> 
<h2>Conclusion</h2> 
<p>In this post, we discussed the different paths and considerations to take into account for live production workflows in the cloud. We determined that choosing which path to leverage is not and/or question – rather, it could be (and often is) a mix of both.</p> 
<p>There are three key considerations that drive the decision making; picture quality, bandwidth, and latency. There is always a tension between the three considerations, depending on what the workflow or application can tolerate, and what are its priorities. For example, if high bandwidth can be made available as needed for uncompressed or JPEG XS, then the outcome is very high picture quality and very low latency. But if available bandwidth is scarcer, then the cost pivots towards the processing required to compress signals (H.264/H.265), introducing higher latency and lower bitrates.</p> 
<p>For more information on some of the services discussed here, best practices, and latest announcements, see the following resources:</p> 
<ul> 
 <li><a href="https://aws.amazon.com/elemental-live/">AWS Elemental Live</a>, <a href="https://aws.amazon.com/mediaconnect/">AWS Elemental MediaConnect</a></li> 
 <li><a href="https://docs.aws.amazon.com/mediaconnect/latest/ug/best-practices.html">Best practices for AWS Elemental MediaConnect</a></li> 
 <li><a href="https://aws.amazon.com/blogs/media/what-aws-cdi-means-for-the-future-of-content-production-and-delivery/">CDI</a>, <a href="https://aws.amazon.com/about-aws/whats-new/2021/05/aws-elemental-mediaconnect-adds-cdi-and-jpeg-xs-support/">JPEGXS support</a></li> 
 <li>Related posts (<a href="https://aws.amazon.com/blogs/media/what-aws-cdi-means-for-the-future-of-content-production-and-delivery/">CDI</a>, <a href="https://aws.amazon.com/about-aws/whats-new/2021/05/aws-elemental-mediaconnect-adds-cdi-and-jpeg-xs-support/">JPEGXS support</a>, <a href="https://aws.amazon.com/blogs/media/cs-smpte-2022-7-with-aws-elemental-live/">Using SMPTE 2022-7 with AWS Elemental Live</a>)</li> 
</ul>
