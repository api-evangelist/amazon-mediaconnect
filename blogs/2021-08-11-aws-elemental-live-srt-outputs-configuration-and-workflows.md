---
title: "AWS Elemental Live SRT outputs configuration and workflows"
url: "https://aws.amazon.com/blogs/media/awse-aws-elemental-live-srt-outputs-configuration-and-workflows/"
date: "Wed, 11 Aug 2021 17:41:09 +0000"
author: "Abbas Nemr"
feed_url: "https://aws.amazon.com/blogs/media/tag/aws-elemental-mediaconnect/feed/"
---
<h2>Introduction</h2> 
<p><a href="https://aws.amazon.com/elemental-live/">AWS Elemental Live</a> version 2.21 introduced support for <a href="https://aws.amazon.com/blogs/media/metfc-srt-inputs-in-aws-elemental-live/">ingesting transport stream (TS) inputs using SRT</a>. Now, version 2.22 supports delivering TS outputs using SRT.</p> 
<p>The SRT (<a href="https://www.srtalliance.org/">Secure Reliable Transport</a>) protocol is an open-source transport technology optimized for live audio/video streaming. SRT enables secure and reliable transport of content across unpredictable, noisy networks, such as the Internet. SRT offers multiple benefits when transporting live video content over the internet:</p> 
<ul> 
 <li>It helps compensate for jitter and bandwidth fluctuations.</li> 
 <li>It minimizes the packet loss.</li> 
 <li>It supports AES encryption to protect content in transit.</li> 
</ul> 
<p>SRT is only one of the delivery content types and protocols that Live supports. For a complete list, see the AWS Elemental Live <a href="https://docs.aws.amazon.com/elemental-live/latest/ug/what-is-aws-elemental-live.html">User Guide</a>.</p> 
<p>In this post, we are presenting current SRT support in Live, with particular attention to setting up a workflow with <a href="https://aws.amazon.com/mediaconnect/">AWS Elemental MediaConnect</a> as an SRT destination. We are also showing a few configuration aspects of SRT outputs in Elemental Live.</p> 
<h2>SRT support in AWS Elemental Live</h2> 
<p>Elemental Live supports ingest of SRT sources in caller mode. It supports delivery of SRT outputs in either caller or listener modes.</p> 
<p>The following Table-1 provides an overview of current SRT protocol support in Live:</p> 
<table> 
 <tbody> 
  <tr> 
   <td width="340"><strong>Feature</strong></td> 
   <td width="142"><strong>SRT Inputs</strong></td> 
   <td width="142"><strong>SRT Outputs</strong></td> 
  </tr> 
  <tr> 
   <td width="340"><strong>Caller mode</strong></td> 
   <td width="142">Yes</td> 
   <td width="142">Yes</td> 
  </tr> 
  <tr> 
   <td width="340"><strong>Listener mode</strong></td> 
   <td width="142"></td> 
   <td width="142">Yes</td> 
  </tr> 
  <tr> 
   <td width="340"><strong>AES Encryption (128, 192, 256)</strong></td> 
   <td width="142">Yes</td> 
   <td width="142">Yes</td> 
  </tr> 
  <tr> 
   <td width="340"><strong>Multiple Audios</strong></td> 
   <td width="142">Yes</td> 
   <td width="142">Yes</td> 
  </tr> 
  <tr> 
   <td width="340"><strong>Dolby Audio</strong></td> 
   <td width="142">Yes</td> 
   <td width="142">Yes</td> 
  </tr> 
  <tr> 
   <td width="340"><strong>Embedded Captions</strong></td> 
   <td width="142">Yes</td> 
   <td width="142">Yes</td> 
  </tr> 
  <tr> 
   <td width="340"><strong>SCTE-35 Markers</strong></td> 
   <td width="142">Yes</td> 
   <td width="142">Yes</td> 
  </tr> 
  <tr> 
   <td width="340"><strong>Seamless ARN integration with </strong><strong>MediaConnect</strong></td> 
   <td width="142">Yes</td> 
   <td width="142">Yes</td> 
  </tr> 
 </tbody> 
</table> 
<h2>Caller and listener, sender and receiver</h2> 
<p>With SRT, the encoder and the downstream destination each have two roles – sender and receiver, and caller and listener.</p> 
<ul> 
 <li>The send and receive roles cover the direction of the transmission of the content. With an SRT output, Elemental Live is always the sender of the output.</li> 
 <li>The call and listen roles cover the handshake protocol. The caller initiates the handshake that precedes transmission. The listener listens for the handshake, and will respond when it receives the handshake request.</li> 
</ul> 
<p>Typically, the decision to configure Live as the caller or as the listener is based on the role of the downstream system. If the downstream system is configured as the listener, you must configure Live as the caller, and vice versa.</p> 
<h2>Elemental Live as SRT caller</h2> 
<p>A typical scenario for setting up Live as the caller is when the downstream destination is a MediaConnect flow. A MediaConnect flow always works in listener mode, so Live must be the caller.</p> 
<p><img alt="Example of SRT caller/listener workflow" class="wp-image-9146 size-full aligncenter" height="452" src="https://d2908q01vomqb2.cloudfront.net/fb644351560d8296fe6da332236b1f8d61b2828a/2021/08/10/Picture1-3.png" width="977" /></p> 
<p>&nbsp;</p> 
<p>When you start the Live event, Live initiates the handshake to AWS Elemental MediaConnect. After the handshake is completed, Live automatically starts streaming video packets to MediaConnect.</p> 
<p>In the following sections we are presenting how to set up the workflow in MediaConnect and Live. The MediaConnect <em>flow</em> and the Elemental Live <em>event</em> must work together.</p> 
<p><em>Note: </em><em>Elemental Live and MediaConnect can be configured seamlessly to work together using the flow ARN; however, in this post we are demonstrating the more generic configuration that will work with any SRT receiver.</em></p> 
<h2>Example of configuration on AWS Elemental MediaConnect (listener) side</h2> 
<p>The following is an illustration of the setup on the AWS Management console of a MediaConnect flow with a Standard source. The fields that relate to SRT are highlighted.</p> 
<p><img alt="MediaConnect flow source configuration, highlighting the Protocol, Inbound port, CIDR block and Minimum latency fields" class="wp-image-9147 size-full aligncenter" height="756" src="https://d2908q01vomqb2.cloudfront.net/fb644351560d8296fe6da332236b1f8d61b2828a/2021/08/10/Picture2-2.png" width="977" /></p> 
<p>&nbsp;</p> 
<p>Consider the following when setting up the flow to listen for SRT requests:</p> 
<ul> 
 <li><strong>Protocol</strong>: select “SRT listener” from the drop-down list</li> 
 <li><strong>Inbound port:</strong> 
  <ul> 
   <li>The SRT protocol transports content via bidirectional UDP. On the SRT listener, you must designate a UDP port where the listener will listen for an inbound request to start the SRT streaming session.</li> 
   <li>Notice that a MediaConnect flow supports any inbound port from 1 to 65535 except 2077 or 2088. In addition, it’s recommended to avoid using system ports 1 to 1023.</li> 
   <li>In our example in figure 2, the inbound port is 5050.</li> 
  </ul> </li> 
 <li><strong>CIDR block</strong>: 
  <ul> 
   <li>Set up MediaConnect so that only your Live node can access the SRT listener flow. You therefore need the public IP address of the Live node.</li> 
   <li>To obtain this address, run the following command through SSH on the node: curl http://checkip.amazonaws.com</li> 
   <li>In the MediaConnect flow, create a CIDR block to allow this address, as shown in figure 2.</li> 
  </ul> </li> 
 <li><strong>Minimum latency</strong>: 
  <ul> 
   <li>The best value to use for latency depends on your workflow. Keep in mind that using higher latency results in higher stability.</li> 
   <li>Configure both the event and the flow with identical latency. In case of latency mismatch between the two, the higher value will be used on both sides.</li> 
  </ul> </li> 
</ul> 
<h2>Content encryption</h2> 
<p>If you want to use AES encryption for the content, keep in mind that SRT uses a passphrase to derive the AES key. MediaConnect uses the passphrase to decrypt content. To give MediaConnect access to the passphrase, you must set it up as a <em>secret</em> in <a href="https://aws.amazon.com/secrets-manager/">AWS Secrets Manager</a>, and you must grant permission for MediaConnect to access the secret, as explained <a href="https://docs.aws.amazon.com/mediaconnect/latest/ug/encryption-static-key-set-up.html">here</a>. Notice that:</p> 
<ul> 
 <li>Both the SRT caller and listener must use the same passphrase.</li> 
 <li>Live supports passphrases with a length of 10 to 72 characters.</li> 
 <li>As an example, we will use the passphrase “AWS is Awesome” on both sides in this post as shown in figures 3 and 5.</li> 
</ul> 
<p>Here is an illustration of the setup for decryption in the MediaConnect flow source. Notice that in MediaConnect, you set up with the Secret ARN. You don’t enter the actual passphrase anywhere in MediaConnect.</p> 
<p><img alt="MediaConnect flow, decryption configuration" class="wp-image-9148 size-full aligncenter" height="464" src="https://d2908q01vomqb2.cloudfront.net/fb644351560d8296fe6da332236b1f8d61b2828a/2021/08/10/Picture3-2.png" width="977" /></p> 
<p>&nbsp;</p> 
<p>After the flow is created, MediaConnect will associate an inbound IP address with the flow. Live will use this address plus the inbound port as the destination, as shown in figure 5.</p> 
<p><img alt="MediaConnect flow, inbound IP address and port" class="wp-image-9149 size-full aligncenter" height="295" src="https://d2908q01vomqb2.cloudfront.net/fb644351560d8296fe6da332236b1f8d61b2828a/2021/08/10/Picture4-2.png" width="977" /></p> 
<h2>Example of configuration on the Elemental Live (caller) side</h2> 
<p>When configuring Live, keep in mind that the on-premises firewall must be configured to allow outbound UDP traffic to MediaConnect flow, and to allow inbound UDP traffic to Live.</p> 
<p>In this example, we haven’t configured for redundancy, so only the fields for the primary destination are completed. Notice how the values we entered follow the preceding guidelines.</p> 
<p><img alt="Live SRT caller output configuration" class="wp-image-9152 size-full aligncenter" height="499" src="https://d2908q01vomqb2.cloudfront.net/fb644351560d8296fe6da332236b1f8d61b2828a/2021/08/10/five.png" width="977" /></p> 
<p>In the Live web interface, consider the following when creating the output:</p> 
<ul> 
 <li>The SRT protocol is available via the “Reliable TS” output group tab.</li> 
</ul> 
<p><img alt="Live UI, Output Groups" class="wp-image-9153 size-full aligncenter" height="129" src="https://d2908q01vomqb2.cloudfront.net/fb644351560d8296fe6da332236b1f8d61b2828a/2021/08/10/six.png" width="977" /></p> 
<p>&nbsp;</p> 
<ul> 
 <li><strong>Delivery Protocol</strong>: Choose “Secure Reliable Transport”. Also note that the <strong>SRT Connection Mode</strong> is “Caller”.</li> 
 <li><strong>Destination</strong>: 
  <ul> 
   <li>The destination, consists of the protocol (srt://), the MediaConnect flow inbound IP address, and the port.</li> 
   <li>The IP address and port exactly match the values you listed in the MediaConnect flow. So, the <em>source</em> on the MediaConnect side is the <em>destination</em> on the Live side.</li> 
  </ul> </li> 
 <li><strong>Encryption </strong>fields: The passphrase is always in plaintext. The passphrase in AWS Secrets Manager and the passphrase you enter must be identical.</li> 
 <li><strong>Latency</strong>: Set to the same value we used for MediaConnect flow latency.</li> 
</ul> 
<h2>Monitoring the workflow</h2> 
<p>After both the MediaConnect flow and the Live event are started, you can monitor the workflow in the MediaConnect console (figure 7).</p> 
<ul> 
 <li>In the Sources section for the flow, the “Source health” turns green and shows as “Connected”.</li> 
 <li>In the Source health metrics page for the flow, the section includes a graph that provides a visual confirmation that the workflow is working as expected. The source bitrate indicates that bitrate coming into the flow.</li> 
</ul> 
<p>In the sample graph in figure 7, the source bitrate shows as 6 mbps. Verify that this adds up. In our example, we know that we set up the output with a video bitrate of 5 mbps. When you add in the audio, the TS overhead, and the SRT control packets, a source bitrate of 6 mbps looks reasonable.</p> 
<p><img alt="MediaConnect flow page, Source bitrate metric graph" class="wp-image-9154 size-full aligncenter" height="581" src="https://d2908q01vomqb2.cloudfront.net/fb644351560d8296fe6da332236b1f8d61b2828a/2021/08/10/seven.png" width="949" /></p> 
<h2>Troubleshooting tips: flow isn’t working</h2> 
<p>If the MediaConnect flow source health stays “Disconnected”, check the following:</p> 
<ul> 
 <li>Verify that there are no typos or extra white spaces in the passphrase stored in AWS Secrets Manager and configured in the Live event. A good indicator of SRT passphrase mismatch could be seen in the Live event logs showing a “connection rejected” message:</li> 
</ul> 
<p>[Log message]</p> 
<p><code>SRT Connect error: Connection setup failure: connection rejected</code></p> 
<ul> 
 <li>Perhaps the public IP address of the Live node is not configured properly in the MediaConnect CIDR block. Or perhaps the on-premises firewall is blocking the traffic. Look at the Live event logs for a “connection time out” messages.</li> 
</ul> 
<p>[Log message]</p> 
<p><code>SRT Connect error: Connection setup failure: connection time out</code></p> 
<h2>Elemental Live as SRT listener</h2> 
<p>A typical scenario for setting up Elemental Live in SRT listener mode is when the downstream destination or receiver is located behind a NAT or firewall. That is, the receiver is not reachable from external networks or the internet. Therefore, the Elemental Live cannot initiate the SRT session handshake as a caller.</p> 
<p>In order to demonstrate this scenario, we will use another Elemental Live node as receiver. In the workflow, you have two Elemental Live nodes, which we will call “Live Sender” and “Live Receiver”.</p> 
<p><img alt="Live nodes configured for SRT listener/caller workflow" class="wp-image-9155 size-full aligncenter" height="527" src="https://d2908q01vomqb2.cloudfront.net/fb644351560d8296fe6da332236b1f8d61b2828a/2021/08/10/eight.png" width="977" /></p> 
<p>&nbsp;</p> 
<p>The Live Receiver is accepting the SRT content as an input. Keep in mind that Elemental Live can only ingest SRT content when it is set up as the caller. Therefore, the Live Receiver node must be the caller and the Live Sender node must be the listener.</p> 
<p>Configure the Live Sender and the Live Receiver so that they work together. Consider the following:</p> 
<ul> 
 <li>The Live Sender is configured to listen to a specific UDP port. In our example in figure 9, we use port 5070.</li> 
 <li>The Live Receiver must have a valid network route to the Live Sender in order to initiate the handshake. In our example in figure 10, the Live Receiver is configured to call the private IP address of the Live Sender.</li> 
 <li>In case the Elemental Live built-in firewall is enabled on the Live Sender, you must add a rule to allow inbound UDP traffic to the UDP port designated for listening for SRT requests. To open the port on the Live Sender firewall, see <a href="https://docs.aws.amazon.com/elemental-live/latest/configguide/config-wrkr-cf-cg-firewall.html">Open ports on the firewall</a>.</li> 
</ul> 
<h2>Example of configuration on the SRT listener (Live Sender) side</h2> 
<p>Here is an illustration of the setup of the SRT listener output in the Live Sender.</p> 
<p><img alt="AWS Elemental Live web interface, configuration of SRT output in listener mode" class="wp-image-9156 size-full aligncenter" height="431" src="https://d2908q01vomqb2.cloudfront.net/fb644351560d8296fe6da332236b1f8d61b2828a/2021/08/10/nine.png" width="977" /></p> 
<p>&nbsp;</p> 
<p>The configuration of SRT output in listener mode is similar to what was discussed previously for the caller mode, except for following two differences:</p> 
<ul> 
 <li>The SRT Connection Mode is now set to “Listener”.</li> 
 <li>For the Primary Destination, Live requires the same URI format with “srt://” prefix. However, what really matters is the inbound UDP port (5070) because upstream system (Live Sender) will be listening for SRT requests on that port on all its network interfaces. Therefore, we use the internal loopback IP address “127.0.0.1” as the destination.</li> 
</ul> 
<h2>Example of configuration on the SRT caller (Live receiver) side</h2> 
<p>Here is an illustration of the setup of the input in Live Receiver. Notice that the IP address “10.13.133.77” in figure 10 is the private IP address of the Live Sender.</p> 
<p><img alt="" class="size-full wp-image-9157 aligncenter" height="241" src="https://d2908q01vomqb2.cloudfront.net/fb644351560d8296fe6da332236b1f8d61b2828a/2021/08/10/ten.png" width="977" /></p> 
<h2>Redundancy workflow using SRT listener mode</h2> 
<p>SRT outputs can be set up in a redundant configuration to implement redundant contribution streams to the downstream system. Configuring redundancy in an SRT caller scenario follows the same model as redundancy with other output types, including RTMP or Zixi outputs.</p> 
<p>The redundancy configuration when Live is the SRT listener follows a different model, which we will now discuss.</p> 
<p>We mentioned earlier that the SRT listener mode is used when the SRT receiver is not reachable by the SRT sender. Another use case for SRT listener mode is for redundancy, or more precisely, distributed output using redundancy. You can set up in SRT listener mode when multiple receiver nodes in the downstream remote system need to ingest the SRT source at a certain point. In other words, when the receiver IP address is changing over time.</p> 
<p><img alt="SRT listener output to enable remote system redundancy" class="wp-image-9158 size-full aligncenter" height="654" src="https://d2908q01vomqb2.cloudfront.net/fb644351560d8296fe6da332236b1f8d61b2828a/2021/08/10/eleven.png" width="977" /></p> 
<p>&nbsp;</p> 
<p>Consider the workflow shown in figure 13 where the downstream system (the on-premises Location B) is an N+M cluster that consists of N primary Live nodes and M backup Live nodes. If the primary SRT receiver node chassis fails, then one the backup nodes will be promoted to replace the failed primary node.</p> 
<p>In this scenario, the receiver node IP address will change, but from the SRT perspective, this change has no impact because the newly promoted receiver node is the party that calls the source to initiate the session.</p> 
<h2>Cleanup</h2> 
<p>If you followed the instructions and implemented the setup described in the “Live as Caller” scenario, make sure that you delete the AWS resources after your testing is completed, to avoid incurring any unnecessary cost in your AWS account.</p> 
<ul> 
 <li>In MediaConnect, stop the flow then delete it.</li> 
 <li>In AWS Secrets Manager, delete the secret where you stored the passphrase.</li> 
</ul> 
<h2>Conclusion</h2> 
<p>In this post, we presented the new SRT outputs feature released in AWS Elemental Live software version 2.22. We demonstrated the configuration of SRT outputs in both caller and listener modes using real life examples. We also discussed how to architect for redundancy using SRT listener outputs. For further details on these topics please get in touch with your AWS Elemental account team.</p> 
<h2>Additional Resources</h2> 
<ul> 
 <li><a href="https://docs.aws.amazon.com/elemental-live/latest/ug/what-is-aws-elemental-live.html">AWS Elemental Live User Guide</a></li> 
 <li>Blog post: <a href="https://aws.amazon.com/blogs/media/metfc-srt-inputs-in-aws-elemental-live/">Getting started with SRT inputs in AWS Elemental Live</a> &nbsp;&nbsp;<strong>&nbsp;</strong></li> 
</ul>
