---
title: "AWS Elemental Live introduces JPEG XS for low-latency, visually lossless contribution to the cloud"
url: "https://aws.amazon.com/blogs/media/aws-elemental-live-introduces-jpeg-xs-for-low-latency-visually-lossless-contribution-to-the-cloud/"
date: "Mon, 27 Sep 2021 17:50:45 +0000"
author: "Jason Ackman"
feed_url: "https://aws.amazon.com/blogs/media/tag/aws-elemental-mediaconnect/feed/"
---
<h2>An introduction to JPEG XS</h2> 
<p>As video consumption increases and content providers move toward higher quality content, bandwidth capacity becomes ever more important. Broadcasters transitioning from traditional serial digital interface (SDI) infrastructure to internet protocol (IP) technology for uncompressed video transport within the broadcast facility follow Society of Motion Picture and Television Engineers (SMPTE) standards like SMPTE ST 2110 or the SMPTE ST 2022-6. Please see the excellent <a href="https://aws.amazon.com/blogs/media/part-1-background-and-key-benefits-of-smpte-st-2110-on-aws-elemental-live/">SMPTE ST 2110 blog</a> written by my colleague, Brian Bedard, part one of a three-part series.</p> 
<p>The IP environment offers bandwidth savings, decreased operational costs, and a common data center. However, a forklift approach from SDI to IP is not feasible. This means that SDI and IP will coexist for some time. As content providers push toward 4K resolution (and in some cases 8K resolution), growing bandwidth requirements puts more stress on current infrastructure. The same limitations exist for contribution to the cloud. This drives the need for lightweight compression that preserves video quality while minimizing encoding/decoding latency. <a href="https://aws.amazon.com/elemental-live/">AWS Elemental Live</a> from Amazon Web Services (AWS)—which lets you encode live video on-premises for events and 24/7 streams—has delivered a feature that accomplishes just that.</p> 
<p>Enter JPEG XS. JPEG XS (ISO/IEC 21122) is a visually lossless, low-complexity, and low-latency video codec. JPEG XS targets visually lossless quality obtained by intraframe encoding and the use of fixed compression ratios. In testing, natural video sequences are visually lossless at ratios of up to 10:1. Some content, like computer-generated imagery (CGI) video, may require a lower ratio. The JPEG XS codec offers constant bitrate that conforms nicely with the SMPTE ST 2110 suite and thus can be carried using SMPTE ST 2110-22—support for compressed video transport.</p> 
<p>JPEG XS offers low complexity that facilitates a smaller hardware footprint without the need for additional memory. The codec can run on both CPU- and GPU-based systems while taking advantage of parallelism to achieve real-time or faster than real-time. This low complexity cuts down on power consumption, reducing energy costs.</p> 
<p>JPEG XS provides extremely low latency. The codec boasts subframe latency from encoding to decoding, but realistically, your transit time and any buffering along the way will add additional latency. In an internal test, we output JPEG XS from AWS Elemental Live on-premises to <a href="https://aws.amazon.com/mediaconnect/">AWS Elemental MediaConnect</a>—a high-quality transport service for live video—then input it back to the on-premises AWS Elemental Live for JPEG XS decoding. The test proved subsecond latency through the entire path. <a href="https://aws.amazon.com/directconnect/">AWS Direct Connect</a>—a cloud service solution that makes it easy to establish a private connection between your premises and AWS—provides a dedicated network path to and from the cloud in this test. The JPEG XS codec’s constant bitrate made the bitstream compatible with IP delivery.</p> 
<h2>Applications of JPEG XS</h2> 
<p>JPEG XS can be used anywhere that uncompressed video is in use. It’s a good fit for low-latency applications like virtual reality / augmented reality, autonomous vehicles, vehicle displays, medical procedures, and mobile devices, to name a few. AWS Elemental Live can also unlock new video over IP workflows. You can use JPEG XS in your SMPTE ST 2110 uncompressed environments within a broadcast facility, and you can use the codec to for remote production of events and to improve live video monitoring.</p> 
<p>Due to bandwidth constraints on entry to AWS cloud infrastructure, JPEG XS lends itself to hybrid video workflows, reducing the throughput strain as customers shift to playout in the cloud. AWS Elemental Live provides the low-latency, visually lossless bridge needed to move uncompressed video to the cloud, working alongside AWS Elemental MediaConnect. And MediaConnect can provide JPEG XS or <a href="https://aws.amazon.com/media-services/resources/cdi/">AWS Cloud Digital Interface</a> (AWS CDI) output for cloud processing. AWS CDI is a network technology that transports high-quality uncompressed video inside AWS.</p> 
<h2>AWS Elemental Live and JPEG XS</h2> 
<p>AWS Elemental Live has released its JPEG XS feature set, which makes it simple for AWS customers to ingest/decode and output/encode to the ISO/IEC 21122 standard. Both the encode and the decode functions support transport of the JPEG XS compressed video stream, and follow the SMPTE ST 2110-22 specification for compressed video transport.</p> 
<p>AWS Elemental Live offers overall support for SMPTE ST 2110 and Networked Media Open Specifications (NMOS) for the Advanced Media Workflow Association (AMWA). It breaks down as follows:</p> 
<h3>AWS Elemental Live SMPTE ST 2110 support</h3> 
<ul> 
 <li>2110-10: System Timing and Definitions</li> 
 <li>2110-20: Uncompressed Active Video, Support for SMPTE ST 2022-7</li> 
 <li>2110-21: Traffic Shaping and Delivery Timing</li> 
 <li>2110-22: Constant Bitrate Compressed Video, Support for SMPTE ST 2022-7</li> 
 <li>2110-30: Pulse-Code Modulation (PCM) Digital Audio</li> 
 <li>2110-31: Audio Engineering Society (AES) Transparent Transport</li> 
 <li>2110-40: SMPTE ST 291-1 Ancillary Data</li> 
</ul> 
<h3>AWS Elemental Live NMOS support</h3> 
<ul> 
 <li>AMWA-IS-04: Discovery and Registration</li> 
 <li>AMWA-IS-05: Connection Management</li> 
</ul> 
<h3>AWS Elemental Live ingress support</h3> 
<p>AWS Elemental Live supports ingest according to SMPTE ST 2110-22, which specifies parameters for the Real-Time Transport Protocol (RTP)–based transport of constant bitrate compressed video over IP networks, referenced to a common reference clock. For resiliency, AWS Elemental Live can ingest <a href="https://aws.amazon.com/blogs/media/cs-smpte-2022-7-with-aws-elemental-live/">SMPTE ST 2022-7</a>–compliant streams.</p> 
<h3>AWS Elemental Live egress support</h3> 
<p>The AWS Elemental Live JPEG XS encoding requires an L800 series appliance with a 25 Gigabit Ethernet (GbE) interface. Standard-definition (SD) and high-definition (HD) resolutions are supported as of AWS Elemental Live version 2.21.3 (general availability). AWS Elemental Live supports compression ratios from 2:1 to 12:1. The 12:1 ratio will provide maximum density in tandem with the lowest bandwidth for contribution to the cloud. The range of compression ratios will help you make sure you can reach the subjective/objective video quality level that you desire while achieving the bandwidth savings provided by the JPEG XS codec. The AWS Elemental Live JPEG XS output is transported according to SMPTE ST 2110-22 specification, and AWS Elemental Live can output SMPTE ST 2022-7–compliant streams.</p> 
<h2>How-to steps for outputting JPEG XS to the cloud and back</h2> 
<p>Now, let’s get our hands dirty. In the example that follows, we output an AWS Elemental Live JPEG XS encoding from on-premises to AWS Elemental MediaConnect for contribution into the cloud. We then take the AWS Elemental MediaConnect JPEG XS output back down to AWS Elemental Live for ingest and distribution.</p> 
<p><img alt="" class="size-full wp-image-9637 aligncenter" height="899" src="https://d2908q01vomqb2.cloudfront.net/fb644351560d8296fe6da332236b1f8d61b2828a/2021/09/23/ThursdaySept23_20211.png" width="977" /></p> 
<p>AWS Elemental Live and AWS Elemental MediaConnect need to be in the same <a href="https://aws.amazon.com/vpc/">Amazon Virtual Private Cloud</a> (Amazon VPC), a service that gives you complete control over your virtual networking environment. AWS Direct Connect facilitates the network path to the cloud. AWS Elemental MediaConnect requires SMPTE ST 2022-7 for JPEG XS ingress and output. The following steps guide you through setting up this workload.</p> 
<h2>AWS Elemental Live JPEG XS encoding</h2> 
<p>Follow these steps to create an AWS Elemental Live event that outputs SMPTE ST 2110-22 with JPEG XS encoding:</p> 
<ul> 
 <li>Create a SMPTE ST 2110 <strong>Output Group</strong>.</li> 
 <li>Add an <strong>Output</strong> for which the <strong>Stream</strong> type is <strong>Video</strong>.</li> 
 <li>For <strong>Video Codec</strong>, select <strong>JPEG XS</strong>.<img alt="" class="size-full wp-image-9638 aligncenter" height="181" src="https://d2908q01vomqb2.cloudfront.net/fb644351560d8296fe6da332236b1f8d61b2828a/2021/09/23/ThursdaySept23_20212.png" width="977" /></li> 
</ul> 
<ul> 
 <li>Select <strong>Advanced</strong> and choose a <strong>Compression Ratio</strong> (1:12 providing the best BW savings).<img alt="" class="size-full wp-image-9639 aligncenter" height="452" src="https://d2908q01vomqb2.cloudfront.net/fb644351560d8296fe6da332236b1f8d61b2828a/2021/09/23/ThursdaySept23_20213.png" width="977" /></li> 
</ul> 
<ul> 
 <li>Add another <strong>Output</strong> for which the <strong>Stream </strong>type is <strong>Audio</strong>.</li> 
 <li>For <strong>Audio Codec</strong>, select <strong>PCM</strong>.</li> 
 <li>Use the remaining defaults.<img alt="" class="size-full wp-image-9640 aligncenter" height="156" src="https://d2908q01vomqb2.cloudfront.net/fb644351560d8296fe6da332236b1f8d61b2828a/2021/09/23/ThursdaySept23_20214.png" width="977" /></li> 
</ul> 
<p>For AWS Elemental MediaConnect, we will require SMPTE ST 2022-7 packet merging.</p> 
<ul> 
 <li>We provide the <strong>Primary and Secondary Destinations</strong> after setting up the AWS Elemental MediaConnect flow.</li> 
 <li>When the AWS Elemental Live event starts, a SMPTE ST 2022-7–compliant Session Description Protocol (SDP) file will be programmatically generated and will be located on the appliance in /opt/elemental_se/web/public/directory.</li> 
</ul> 
<h2>AWS Elemental MediaConnect JPEG XS ingest</h2> 
<p>Follow these steps to set up AWS Elemental MediaConnect JPEG XS Input:</p> 
<ul> 
 <li>Open the AWS Elemental MediaConnect console at <a href="https://console.aws.amazon.com/mediaconnect/">https://console.aws.amazon.com/mediaconnect/</a></li> 
 <li>On the <strong>Flows</strong> page, choose <strong>Create flow</strong>.</li> 
 <li>In the <strong>Details</strong> section, for <strong>Name</strong>, specify a name for your flow. This name will become part of the Amazon Resource Name (ARN) for this flow.</li> 
 <li>For <strong>Availability Zone</strong>, choose the Availability Zone where your VPC subnet resides.</li> 
 <li>In the <strong>Source</strong> section, for <strong>Source type</strong>, choose <strong>VPC source</strong>.</li> 
 <li>For <strong>Name</strong>, specify a name for your source. This value is an identifier that is visible only on the AWS Elemental MediaConnect console.</li> 
 <li>Skip to the <strong>VPC interface</strong> section.</li> 
 <li>For each VPC that you want to connect to the flow, do the following: 
  <ul> 
   <li>Choose <strong>Add VPC interface</strong>. 
    <ul> 
     <li>For <strong>Name</strong>, specify a name for your VPC interface. The name of the VPC interface must be unique within the flow. We’ll refer to this as “VPCa.”</li> 
     <li>For <strong>Type</strong>, choose <strong>EFA</strong> as the type for VPCa and <strong>ENA </strong>for VPCb.</li> 
     <li>For <strong>Role ARN</strong>, specify the ARN of the role that you created when you set up AWS Elemental MediaConnect as a trusted service.</li> 
     <li>For <strong>VPC</strong>, choose the identifier of the VPC that you want to use.</li> 
     <li>For <strong>Subnet</strong>, choose the VPC subnet that you want AWS Elemental MediaConnect to use to set up your VPC configuration. You can choose as many as you want, but you must choose at least one.</li> 
     <li>For <strong>Security groups</strong>, specify the VPC security groups that you want AWS Elemental MediaConnect to use to set up your VPC configuration. You must choose at least one security group.</li> 
    </ul> </li> 
   <li>Add another VPC interface, and specify a name. We’ll refer to this as “VPCb.” Repeat the same steps to complete the VPC interface setup.<img alt="" class="size-full wp-image-9641 aligncenter" height="729" src="https://d2908q01vomqb2.cloudfront.net/fb644351560d8296fe6da332236b1f8d61b2828a/2021/09/23/ThursdaySept23_20215.png" width="977" /><img alt="" class="size-full wp-image-9642 aligncenter" height="683" src="https://d2908q01vomqb2.cloudfront.net/fb644351560d8296fe6da332236b1f8d61b2828a/2021/09/23/ThursdaySept23_20216.png" width="977" /></li> 
  </ul> </li> 
</ul> 
<ul> 
 <li>Next, create your media streams. 
  <ul> 
   <li>In the <strong>Media streams</strong> section, choose <strong>Add media stream</strong>.</li> 
   <li>In the <strong>Name</strong> field, specify a descriptive name that will help you distinguish this media stream from others in the flow.</li> 
   <li>For <strong>Description</strong>, specify a description that will help you remember the use of this media stream.</li> 
   <li>For <strong>Stream ID</strong>, specify a unique identifier for the media stream.</li> 
   <li>Choose <strong>Advanced options</strong> to display the additional options based on your stream type.</li> 
   <li>For <strong>Stream type</strong>, choose <strong>Video</strong>.</li> 
   <li><strong>Media clock rate</strong> is the sample rate for the stream and is set to 90000. This value is measured in kilohertz (kHz).</li> 
   <li>For <strong>Video format</strong>, specify the resolution of the video.</li> 
   <li>For <strong>Exact framerate</strong>, specify the frame rate of the video. This value should be represented in frames per second.</li> 
   <li>For <strong>Colorimetry</strong>, specify the format that was used for the representation of color in the video.</li> 
   <li>For <strong>Scan mode</strong>, specify the method that was used to scan the incoming video. 
    <ul> 
     <li>Choose <strong>Interlace</strong> if the incoming video is interlaced (for example, 480i or 1080i).</li> 
     <li>Choose <strong>Progressive</strong> if the incoming video is progressive (for example, 720p or 1080p).</li> 
     <li>Choose <strong>Progressive segmented frame</strong> (PSF) if the incoming video is PSF (for example, 1080psf).</li> 
    </ul> </li> 
   <li>For <strong>TCS</strong>, specify the transfer characteristic system (TCS) that was used in the video.</li> 
   <li>For <strong>Range</strong>, specify the encoding range of the video.</li> 
   <li>For <strong>PAR</strong>, specify the pixel access ratio (PAR) of the video.</li> 
   <li>Choose <strong>Add media stream</strong>.<img alt="" class="size-full wp-image-9643 aligncenter" height="854" src="https://d2908q01vomqb2.cloudfront.net/fb644351560d8296fe6da332236b1f8d61b2828a/2021/09/23/ThursdaySept23_20217.png" width="977" /></li> 
  </ul> </li> 
</ul> 
<ul> 
 <li>Next, add the audio. 
  <ul> 
   <li>Select <strong>Add media stream</strong>.</li> 
   <li>In the <strong>Name</strong> field, specify a descriptive name that will help you distinguish this media stream from others in the flow.</li> 
   <li>For <strong>Description</strong>, specify a description that will help you remember the use of this media stream.</li> 
   <li>For <strong>Stream ID</strong>, specify a unique identifier for the media stream.</li> 
   <li>Choose <strong>Advanced options</strong> to display the additional options based on your stream type.</li> 
   <li>For <strong>Stream type</strong>, choose <strong>Audio</strong>.</li> 
   <li>For <strong>Media clock rate</strong>, the default value of <strong>48000 kHz</strong>, which will match our AWS Elemental Live configuration.</li> 
   <li>Choose <strong>Add media stream</strong>.<img alt="" class="size-full wp-image-9644 aligncenter" height="614" src="https://d2908q01vomqb2.cloudfront.net/fb644351560d8296fe6da332236b1f8d61b2828a/2021/09/23/ThursdaySept23_20218.png" width="977" /></li> 
  </ul> </li> 
</ul> 
<ul> 
 <li> 
  <ul> 
   <li>Scroll back up to the <strong>Sources</strong> section.</li> 
   <li>For <strong>Protocol</strong>, choose <strong>ST 2110 with JPEG XS</strong>.</li> 
   <li>For <strong>Max sync buffer</strong>, specify the size of the buffer that you want AWS Elemental MediaConnect to use to sync incoming source data. This value is measured in milliseconds (ms).</li> 
   <li>For <strong>VPC interface name 1</strong>, choose VPCa as a source.</li> 
   <li>For <strong>VPC interface name 2</strong>, choose VPCb as a source. There is no priority between VPC interfaces 1 and 2.</li> 
   <li>For each media stream that you want to use as part of the source, do the following: 
    <ul> 
     <li>For <strong>Media stream name</strong>, choose the name of the media stream.</li> 
     <li>For <strong>Encoding name</strong>, accept the default value. 
      <ul> 
       <li>For ancillary data streams, the encoding name is smpte291.</li> 
       <li>For audio streams, the encoding name is pcm.</li> 
       <li>For video, the encoding name is jxsv.</li> 
      </ul> </li> 
     <li>For <strong>Inbound port</strong>, specify the port that the flow will listen on for incoming content. This value can be anything from 1024 to 65535, with the exception of 2077 and 2088 (those ports are reserved for other protocols).<img alt="" class="size-full wp-image-9645 aligncenter" height="854" src="https://d2908q01vomqb2.cloudfront.net/fb644351560d8296fe6da332236b1f8d61b2828a/2021/09/23/ThursdaySept23_20219.png" width="977" /></li> 
    </ul> </li> 
  </ul> </li> 
</ul> 
<ul> 
 <li>Hit the <strong>Create Flow</strong> button.</li> 
 <li>Next, under the <strong>Sources</strong>, scroll down to <strong>Input Configurations</strong> and find the two IP addresses for each VPC.</li> 
 <li>Now, go back to the AWS Elemental Live <strong>Output Configuration</strong>. Update the <strong>Primary and Secondary Destinations</strong> with the two VPC IPs. 
  <ul> 
   <li>Make sure to use the 25 GbE interface for your outputs.<img alt="" class="size-full wp-image-9646 aligncenter" height="339" src="https://d2908q01vomqb2.cloudfront.net/fb644351560d8296fe6da332236b1f8d61b2828a/2021/09/23/ThursdaySept23_202110.png" width="977" /></li> 
  </ul> </li> 
</ul> 
<ul> 
 <li><strong>Start </strong>your AWS Elemental MediaConnect flow and start your AWS Elemental Live event.</li> 
</ul> 
<p>Now we have JPEG XS video in the cloud. Using AWS Elemental MediaConnect, we can take AWS CDI out to <a href="https://aws.amazon.com/medialive/">AWS Elemental MediaLive</a>—a broadcast-grade live video processing service—or any third-party playout system that supports AWS CDI.</p> 
<p>MediaConnect can also output JPEG XS. If you have a playout system that originates in the cloud, the signal can be brought back on-premises using and AWS Elemental Live. We will use the MediaConnect flow we just created to illustrate this point.</p> 
<h2>AWS Elemental MediaConnect JPEG XS output</h2> 
<p>Follow these steps to create an AWS Elemental MediaConnect Output:</p> 
<ul> 
 <li>Choose the <strong>Output</strong> tab in your MediaConnect <strong>Flow</strong>.</li> 
 <li>In the <strong>Name</strong> field, specify a descriptive name that will help you distinguish this output from others in the flow.</li> 
 <li>For <strong>Protocol</strong>, choose <strong>ST 2110 with JPEG XS</strong>.</li> 
 <li>For <strong>VPC Interface 1</strong>, choose <strong>VPCa</strong> from the drop-down menu.</li> 
 <li>For <strong>VPC Interface 2</strong>, choose <strong>VPCb</strong> from the drop-down menu.</li> 
 <li>Provide the AWS Elemental Live 25 GbE redundant interface IP addresses for the <strong>Destination IP addresses</strong>.<img alt="" class="size-full wp-image-9647 aligncenter" height="858" src="https://d2908q01vomqb2.cloudfront.net/fb644351560d8296fe6da332236b1f8d61b2828a/2021/09/23/ThursdaySept23_202111.png" width="977" /></li> 
</ul> 
<ul> 
 <li>Scroll down to <strong>Media Stream Configuration</strong>, and choose <strong>Add Media Stream Configuration</strong>.</li> 
 <li>Select your <strong>Video Media Stream</strong> for <strong>Media Stream Name</strong>.</li> 
 <li>Provide the <strong>Destination Port</strong>. We will use 5001 in this example.</li> 
 <li>Choose <strong>Add</strong> <strong>Media Stream Configuration</strong>. We will add our audio here.</li> 
 <li>Select your <strong>Audio Media Stream</strong> for <strong>Media Stream Name</strong>.</li> 
 <li>Provide the <strong>Destination Port</strong>. We will use 5002 in this example. The ports need to be unique.</li> 
 <li>Hit the <strong>Save</strong> button.<img alt="" class="size-full wp-image-9648 aligncenter" height="879" src="https://d2908q01vomqb2.cloudfront.net/fb644351560d8296fe6da332236b1f8d61b2828a/2021/09/23/ThursdaySept23_202112.png" width="977" /></li> 
</ul> 
<ul> 
 <li>For SDP creation, we will use an AWS Elemental MediaConnect tool called sdp_converter.py, available on GitHub. 
  <ul> 
   <li>The sdp_converter.py script converts AWS Elemental MediaConnect JSON-formatted data to an SDP file compatible with the SMPTE ST 2110-20/-22/-30/-31/-40 specifications. The script ingests a JSON file that encapsulates the AWS Elemental MediaConnect stream data.</li> 
  </ul> </li> 
</ul> 
<h2>AWS Elemental Live JPEG XS decoding</h2> 
<p>Follow these steps to create an AWS Elemental Live event that inputs SMPTE ST 2110-22 with JPEG XS decoding:</p> 
<ul> 
 <li>Provide <strong>SDP Location</strong>, <strong>Media Index</strong>, and <strong>Interface</strong> values for the following sources. 
  <ul> 
   <li><strong>Video SDP Location</strong> (required)</li> 
   <li><strong>Audio SDP Location</strong> (required)</li> 
   <li><strong>Ancillary SDP Location</strong> (optional)<img alt="" class="size-full wp-image-9649 aligncenter" height="108" src="https://d2908q01vomqb2.cloudfront.net/fb644351560d8296fe6da332236b1f8d61b2828a/2021/09/23/ThursdaySept23_202113.png" width="977" /></li> 
  </ul> </li> 
</ul> 
<ul> 
 <li>If you’re using <strong>NMOS</strong>, tick the<strong> NMOS Control</strong> box.</li> 
 <li>Provide the <strong>Video and Audio Stream Interfaces</strong>. We will use <strong>eth4</strong> and <strong>eth5</strong> <strong>Interfaces</strong> for SMPTE ST 2022-7 ingestion.<img alt="" class="size-full wp-image-9650 aligncenter" height="435" src="https://d2908q01vomqb2.cloudfront.net/fb644351560d8296fe6da332236b1f8d61b2828a/2021/09/23/ThursdaySept23_202114.png" width="977" /></li> 
</ul> 
<ul> 
 <li>Note: Resolution and frame rate support is limited to the resolutions and frame rate support on any SDI interface.</li> 
 <li>Create the output type you desire and hit <strong>Start</strong>.</li> 
</ul> 
<p>You now have video/audio back on-premises. Process and distribute as you wish.</p> 
<h2>Summary</h2> 
<p>In this post we talked about the stress on infrastructure created by higher resolutions and higher frame rates. This creates the need for a visually lossless, low-complexity, and low-latency codec. JPEG XS provides these qualities and is a good solution anywhere uncompressed video over IP is in use. By using JPEG XS, you can enrich many workflows.</p> 
<p>We are excited that AWS Elemental Live and AWS Elemental MediaConnect facilitate workflows requiring SMPTE ST 2110 and contribution to the cloud on AWS for remote production, cloud playout, or cloud processing. Elemental Live and MediaConnect also provide contribution from the cloud back to on-premises. To learn more, please refer to <a href="https://docs.aws.amazon.com/mediaconnect/latest/ug/use-cases-cdi.html">the MediaConnect documentation</a>.</p> 
<p>For more information, please check out the AWS Elemental Live documentation for <a href="https://docs.aws.amazon.com/elemental-live/latest/ug/SMPTE-ST-2110.html">Working with SMPTE ST 2110</a>. We look forward to working with our customers on video workflows that JPEG XS unlocks.</p>
