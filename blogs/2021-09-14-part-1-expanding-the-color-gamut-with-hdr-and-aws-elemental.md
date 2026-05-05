---
title: "Part 1: Expanding the color gamut with HDR and AWS Elemental"
url: "https://aws.amazon.com/blogs/media/part-1-expanding-the-color-gamut-with-hdr-and-aws-elemental/"
date: "Tue, 14 Sep 2021 21:27:47 +0000"
author: "Brian Bedard"
feed_url: "https://aws.amazon.com/blogs/media/tag/aws-elemental-mediaconnect/feed/"
---
<p><em>In this three part series, we discuss the current status of High Dynamic Range (HDR) video support across the AWS Elemental Media Services and appliances. Since our founding in 2006, our customers have expected our products to continue to produce the best quality video per bit using the latest codecs and formats. HDR is now next in line for many of our customers. The release of HDR in 2015 was a major milestone for the perceived quality of the video image, but the adoption needed time to allow for the refresh cycles of consumer HDTVs, and for the integrations into the broadcast, production, and post-production tools alongside the DASH, HLS, ITU, ARIB, and SMPTE specifications that are used widely in the media and entertainment industry. With Dolby Vision support in AWS Elemental Live, this is the year when integrating HDR into your live and video-on-demand (VOD) workflows is essential.</em></p> 
<p><strong>Part 1: Expanding the color gamut with HDR and AWS Elemental (this post)</strong><br /> <a href="https://aws.amazon.com/blogs/media/part-2-4k-hdr-vod-workflows-using-aws-elemental-mediaconvert-and-aws-elemental-server/"><strong>Part 2: HDR VOD Workflows using AWS Elemental Server and AWS Elemental </strong><strong>MediaConvert </strong></a><br /> <a href="https://aws.amazon.com/blogs/media/part-3-live-and-vod-to-live-hdr-workflows-on-aws/" rel="noopener noreferrer"><strong>Part 3: Live and VOD-to-Live HDR </strong><strong>Workflows</strong><strong> on </strong><strong>AWS</strong></a></p> 
<h2><strong>What is HDR?</strong></h2> 
<p>HDR content provides more information about brightness and colors across a much wider range. That means HDR content provides brighter whites, utilizes the absolute black that the display can produce, and takes advantage of more shades of gray in between. Similarly, HDR content provides more vivid reds, greens, and blues.</p> 
<p>At its core, HDR has four main implementations: HDR10, HDR10+, HLG, and Dolby Vision. They are all built upon two transfer functions, the perceptual quantizer (PQ or ST.2084) or hybrid log-gamma (HLG or ARIB STD-B67). These transfer functions determine how the cameras capture the content/light and allows a content producer to determine how a display presents the content to the viewer. PQ uses embedded metadata, and HLG uses a backwards compatible equation so the same content can be shown in HDR or SDR (Standard Dynamic Range), which is ideal for over-the-air broadcast where frequencies are limited, such as ATSC 3.0.</p> 
<p>Many TVs and devices support multiple versions of HDR, including both HLG and the metadata-based formats. The biggest difference between HDR10, compared to HDR10+ or Dolby Vision, is the type of metadata presented. With HDR10, static metadata is used, which only allows a single determination of min/max light of the entire asset, as opposed to the dynamic metadata of HDR10+ and Dolby Vision, which can allow for scene-by-scene or even frame-by-frame metadata changes. Dolby Vision is proprietary to Dolby Laboratories, Inc, so there is a cost involved on the manufacturer’s side, which is why not all displays support it in 2021.</p> 
<p>HDR requires 10-bit video depth or greater, which allows the wider color gamut of Rec. 2020 (BT.2020) to be used. Along with the color gamut, the brightness and darkness range supported by HDR provides the additional volume to differentiate the image from SDR and allows up to 10,000 nits of peak brightness, which is drastically more than SDR.</p> 
<p>The following table shows a high-level technical overview of the major differences between three specifications for HD, UHD, and HDR content.</p> 
<p>&nbsp;</p> 
<table> 
 <tbody> 
  <tr style="background-color: #ff9933;"> 
   <td></td> 
   <td style="padding: 5px;"><strong>Rec. 709</strong></td> 
   <td style="padding: 5px;"><strong>Rec. 2020</strong></td> 
   <td style="padding: 5px;"><strong>Rec. 2100</strong></td> 
  </tr> 
  <tr style="background-color: #ffd9b3;"> 
   <td style="padding: 5px;"><strong>Definition</strong></td> 
   <td style="padding: 5px;">HD SDR</td> 
   <td style="padding: 5px;">UHD SDR</td> 
   <td style="padding: 5px;">UHD HDR</td> 
  </tr> 
  <tr style="background-color: #fff2e6;"> 
   <td style="padding: 5px;"><strong>Transfer Function</strong></td> 
   <td style="padding: 5px;">Gamma</td> 
   <td style="padding: 5px;">Gamma</td> 
   <td style="padding: 5px;">PQ or HLG</td> 
  </tr> 
  <tr style="background-color: #ffd9b3;"> 
   <td style="padding: 5px;"><strong>Resolutions</strong></td> 
   <td style="padding: 5px;">1920×1080</td> 
   <td style="padding: 5px;">3840×2160 or 7680×4320</td> 
   <td style="padding: 5px;">1920×1080 or 3840×2160 or 7680×4320</td> 
  </tr> 
  <tr style="background-color: #fff2e6;"> 
   <td style="padding: 5px;"><strong>Frame Rates</strong></td> 
   <td style="padding: 5px;">23.976 – 60</td> 
   <td style="padding: 5px;">23.976 – 120</td> 
   <td style="padding: 5px;">23.976 – 120</td> 
  </tr> 
  <tr style="background-color: #ffd9b3;"> 
   <td style="padding: 5px;"><strong>Interlaced/Progressive</strong></td> 
   <td style="padding: 5px;">Both</td> 
   <td style="padding: 5px;">Progressive Only</td> 
   <td style="padding: 5px;">Progressive Only</td> 
  </tr> 
  <tr style="background-color: #fff2e6;"> 
   <td style="padding: 5px;"><strong>Bit Depth</strong></td> 
   <td style="padding: 5px;">8 or 10</td> 
   <td style="padding: 5px;">10 or 12</td> 
   <td style="padding: 5px;">10 or 12</td> 
  </tr> 
  <tr style="background-color: #ffd9b3;"> 
   <td style="padding: 5px;"><strong>Pixel Aspect Ratio</strong></td> 
   <td style="padding: 5px;">Square</td> 
   <td style="padding: 5px;">Square</td> 
   <td style="padding: 5px;">Square</td> 
  </tr> 
  <tr style="background-color: #fff2e6;"> 
   <td style="padding: 5px;"><strong>Peak Brightness</strong></td> 
   <td style="padding: 5px;">100 nits</td> 
   <td style="padding: 5px;">100 nits</td> 
   <td style="padding: 5px;">10,000 nits (theoretical limit)</td> 
  </tr> 
  <tr style="background-color: #ffd9b3;"> 
   <td style="padding: 5px;"><strong>Color Gamut</strong></td> 
   <td style="padding: 5px;">Standard</td> 
   <td style="padding: 5px;">Wide</td> 
   <td style="padding: 5px;">Wide</td> 
  </tr> 
  <tr style="background-color: #fff2e6;"> 
   <td style="padding: 5px;"><strong>Colors Available</strong></td> 
   <td style="padding: 5px;">Over 16 Million (8 bit)</td> 
   <td style="padding: 5px;">Over 1 Billion (10 bit)</td> 
   <td style="padding: 5px;">Over 1 Billion (10 bit)</td> 
  </tr> 
 </tbody> 
</table> 
<h2><strong>HDR </strong><strong>products overview&nbsp;</strong></h2> 
<p>This series reviews some sample workflows, and dives deep into the settings in each service to help get you start using your HDR content on AWS and creating HEVC outputs that look and feel exceptional. Whether it’s streaming live esports to YouTube in UHD with HDR, or mastering VOD assets for an OTT offering, we will make recommendations on which products may fit your workflows.</p> 
<h2><strong>VOD</strong></h2> 
<p>For video-on-demand workflows, we explore <a href="https://aws.amazon.com/elemental-server/">AWS Elemental Server</a> and <a href="https://aws.amazon.com/mediaconvert/">AWS Elemental MediaConvert</a>. Both on-premises and cloud-based services are available and can also be used to prepare HDR content for use in a VOD-to-live workflow.</p> 
<h2><strong>Live</strong></h2> 
<p>The on-premises product, <a href="https://aws.amazon.com/elemental-live/">AWS Elemental Live</a>, features our most advanced implementation of HDR for live video, supporting inputs and outputs of HLG, HDR10 or even Dolby Vision (profiles 5 and 8.1). <a href="https://aws.amazon.com/medialive/">AWS Elemental MediaLive</a> is our managed live transcoding service that also offers the ability to convert from HDR to SDR for cloud-based workflows where an SDR adaptive-bitrate ladder is also needed from an HDR source. HDR content can be fed to MediaLive via our newest appliance, <a href="https://aws.amazon.com/medialive/features/link/">AWS Elemental Link UHD</a>, which takes either an HDMI or a 12G-SDI source.</p> 
<h2><strong>Origination and transport</strong></h2> 
<p>Whether it’s live or VOD, <a href="https://aws.amazon.com/mediastore/">AWS Elemental MediaStore</a> can serve as your origin for your CDN, such as <a href="https://aws.amazon.com/cloudfront/">Amazon CloudFront</a>. <a href="https://aws.amazon.com/mediaconnect/">AWS Elemental MediaConnect</a> serves as your live video transport to move your content in and out of AWS securely and reliably. These services act as a passthrough and don’t alter your HDR content. <a href="https://aws.amazon.com/mediapackage/" rel="noopener noreferrer" target="_blank">AWS Elemental MediaPackage</a> can also be used as an origin for more demanding workflows that require time delays, DRM, or harvesting by passing an HLS ABR ladder that uses MPEG transport streams from Live or MediaLive. The output from MediaPackage will include fragmented MP4s to comply with the HLS and DASH specifications.</p> 
<h2><strong>Summary</strong></h2> 
<p>In this post, we gave an overview of HDR and the products that we explore in the next two parts of this series. The workflows provided are to help you get started on your HDR journey, as well as dive into the reasoning behind the settings. In the second post of this series, we will dive into how AWS recommends utilizing HDR in VOD workflows.</p>
