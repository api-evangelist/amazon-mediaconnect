---
title: "Introducing AWS Elemental MediaConnect inputs for AWS Elemental Live appliances"
url: "https://aws.amazon.com/blogs/media/introducing-aws-elemental-mediaconnect-inputs-for-aws-elemental-live-appliances/"
date: "Thu, 22 Apr 2021 21:26:44 +0000"
author: "Steve Ward"
feed_url: "https://aws.amazon.com/blogs/media/tag/aws-elemental-mediaconnect/feed/"
---
<p><a href="https://aws.amazon.com/elemental-live/" rel="noopener noreferrer" target="_blank">AWS Elemental Live</a>&nbsp;2.21.2 GA introduces a new input type for use with <a href="https://aws.amazon.com/mediaconnect/?nc2=h_ql_prod_ms_emc" rel="noopener noreferrer" target="_blank">AWS Elemental MediaConnect</a> that makes it easier to securely and reliably ingest content. In this article, I review use cases where MediaConnect inputs are helpful, and show how to configure both MediaConnect and Elemental Live to utilize this capability.</p> 
<p>This new input type utilizes the <a href="https://aws.amazon.com/blogs/media/prmbp-aws-elemental-mediaconnect-adds-srt-to-list-of-high-availability-protocols/" rel="noopener noreferrer" target="_blank">SRT protocol</a> to provide a robust, secure transmission from cloud to ground. In this implementation, Elemental Live acts as the “caller” and MediaConnect as the “listener” as defined in the SRT specification.</p> 
<h2>Cloud to ground workflows</h2> 
<p>An intrinsic part of <a href="https://aws.amazon.com/media-services/" rel="noopener noreferrer" target="_blank">AWS Media Services</a> is the contribution of on-premises video to the AWS Cloud for further processing and distribution. However, there are also instances when video needs to go from cloud to on-premises. For instance, distribution from a content provider or an intra-company workflow from ground to cloud and back again. In many cases customers need to bring processed video back to an on-site location for distribution, transmission, or monitoring. This workflow can also include cloud-based processing and distribution, represented below.</p> 
<div class="wp-caption aligncenter" id="attachment_8027" style="width: 889px;">
 <img alt="Cloud-based processing and distribution workflow " class="wp-image-8027 size-full" height="460" src="https://d2908q01vomqb2.cloudfront.net/fb644351560d8296fe6da332236b1f8d61b2828a/2021/04/22/Picture1-10.png" width="879" />
 <p class="wp-caption-text" id="caption-attachment-8027"><em>Cloud-based processing and distribution workflow </em></p>
</div> 
<h2>Requirements</h2> 
<p>To set up a workflow from MediaConnect to an Elemental Live appliance requires the following. I describe how to retrieve/create this information in the steps that follow.</p> 
<ul> 
 <li>MediaConnect flow details — The flow ARN and output ARN are required inputs to Elemental Live for access</li> 
 <li>AWS credential pair for Elemental Live to access MediaConnect</li> 
 <li>Accessible IP address for the ground encoder — if using public internet, this will likely be the IP address(es) of a public firewall or router, and not the actual IP address of the Elemental Live unit.</li> 
</ul> 
<h2>Overview</h2> 
<p>Step 1: Determine IP address</p> 
<p>Step 2: Create MediaConnect output</p> 
<p>Step 3: Obtain credential pair</p> 
<p>Step 4: Configure Elemental Live input</p> 
<h2>Step 1: Determine IP address</h2> 
<p>There are two possible network configurations you can use to receive your MediaConnect flow into Elemental Live:</p> 
<ol> 
 <li><a href="https://aws.amazon.com/vpc/?vpc-blogs.sort-by=item.additionalFields.createdDate&amp;vpc-blogs.sort-order=desc" rel="noopener noreferrer" target="_blank">Amazon Virtual Private Cloud (VPC)</a> over <a href="https://aws.amazon.com/directconnect/" rel="noopener noreferrer" target="_blank">AWS Direct Connect</a>: This configuration allows your on-premises network to attach to AWS with a secure, dedicated communications channel. Using this approach requires additional configuration and setup on your network, as well as configuration of a VPC interface on MediaConnect. Setting up these additional items are outside the scope of this article, although the rest of the configuration steps are similar.</li> 
 <li>Public internet connection: This configuration uses your existing, internet-accessible network to access AWS via publicly accessible addresses. This is the method I describe in this post.</li> 
</ol> 
<p>To determine the IP address(es) your appliance uses to access AWS, you need to know a little bit about your network. If you have access to a system with a web browser on the same subnet as your Elemental Live appliance, it is possible to identify the public IP address of your network using a search engine. However, if your network uses multiple IP addresses for public internet access, this might not be a reliable indication. Check with your network administrator to confirm if there is a range of IP addresses that your encoder might use. This range would be expressed in CIDR notation and can be used directly with MediaConnect.</p> 
<p><strong>Important</strong>: AWS Elemental strongly recommends placing your Elemental Live appliance behind a suitable network firewall and router to prevent direct access from the general internet.</p> 
<h2>Step 2 — Create MediaConnect output</h2> 
<p>For the purpose of this article, I assume the MediaConnect flow that provides the feed already exists. As a result, the only thing to do on MediaConnect is create the output that Elemental Live accesses.</p> 
<p>a. In the MediaConnect console, locate the flow you want to use and click on the name to open the flow properties page.</p> 
<p>b. Halfway down the page, choose the Outputs tab.</p> 
<p>c. Choose <strong>Add output</strong>. The Add output dialog appears.</p> 
<p>d. Assign a unique name to the output, and choose the appropriate output type.</p> 
<p>e. For <strong>Protocol</strong>, choose <strong>SRT listener</strong>.</p> 
<p>f. Optionally, add a description.</p> 
<p>g. Set the maximum latency for the output in milliseconds.</p> 
<p>h. Choose a port number for the SRT traffic. Values between 1024 and 65535 are legal, except for ports 2077 and 2088. Check with your network administrator in case there are additional restrictions for your firewall.</p> 
<p>i. Provide the CIDR range(s) from step 1, above, in the <strong>CIDR allow list</strong> field. If you have more than one range to reference, you can add up to three via the <strong>Add</strong> button below the field.</p> 
<p>j. If you want to utilize encryption, proceed to the <strong>Encryption</strong> section of the dialog and tick the <strong>Enable</strong> box. The <strong>Role ARN</strong> and <strong>Secret ARN</strong> drop-down boxes appear, to allow you to specify which secret contains the encryption key (see MediaConnect documentation on setting up static key encryption here)</p> 
<p>k. Choose <strong>Add output</strong>. In a few seconds the output is added to the list for the flow.</p> 
<div class="wp-caption aligncenter" id="attachment_8028" style="width: 889px;">
 <img alt="Output details " class="wp-image-8028 size-full" height="822" src="https://d2908q01vomqb2.cloudfront.net/fb644351560d8296fe6da332236b1f8d61b2828a/2021/04/22/Picture2-5.png" width="879" />
 <p class="wp-caption-text" id="caption-attachment-8028"><em>Output details </em></p>
</div> 
<p>&nbsp;</p> 
<p>Copy the Flow ARN and Output ARN for this output. You need them for your input definition in Elemental Live.</p> 
<h2>Step 3 — Obtain credential pair</h2> 
<p>If you have an IAM administrator, obtain the IAM credential from them. The user requires read-only permissions for secretsmanager (GetResourcePolicy, GetSecretValue, DescribeSecret, ListSecretVersionIds), mediaconnect (DescribeFlow, ListFlows, ListEntitlements), and iam:GetRole. The user credentials are in the form of an Access Key ID and a Secret Key.</p> 
<h2>Step 4 — Configure Elemental Live input</h2> 
<p>Create or edit an event as usual. For your input, select <a href="https://aws.amazon.com/mediaconnect/" rel="noopener noreferrer" target="_blank">AWS Elemental MediaConnect</a>.&nbsp;The input fields appear as below:</p> 
<div class="wp-caption aligncenter" id="attachment_8029" style="width: 889px;">
 <img alt="Elemental Live input " class="wp-image-8029 size-full" height="145" src="https://d2908q01vomqb2.cloudfront.net/fb644351560d8296fe6da332236b1f8d61b2828a/2021/04/22/Picture3-3.png" width="879" />
 <p class="wp-caption-text" id="caption-attachment-8029"><em>Elemental Live input </em></p>
</div> 
<p>a. For <strong>Flow ARN</strong>, copy the ARN of the MediaConnect flow from step 2 above.</p> 
<p>b. For <strong>Output ARN</strong>, copy the ARN from the SRT listener output you created.</p> 
<p>c. For <strong>Interface</strong>, optionally enter the name of the network interface you want to use to initiate the connection to MediaConnect. If left blank, the system routing table will determine which interface is used.</p> 
<p>d. For A<strong>ccess Key ID/Secret Access Key</strong>, use the IAM credential pair obtained in Step 3.</p> 
<p>Once this information is completed, (and providing the MediaConnect flow is in ACTIVE state), the preview button for the input should be able to access and display the source.</p> 
<h2>Summary</h2> 
<p>In this post I demonstrated how to configure an input on Elemental Live using the new MediaConnect input type. I reviewed the steps in configuring the output in the MediaConnect console, and explained how to gather the needed information for a successful delivery of content. Full documentation for MediaConnect is available <a href="https://docs.aws.amazon.com/mediaconnect/" rel="noopener noreferrer" target="_blank">here</a>, and Elemental Live documentation is <a href="https://docs.aws.amazon.com/elemental-live/index.html" rel="noopener noreferrer" target="_blank">here</a>.</p> 
<p>&nbsp;</p> 
<p>&nbsp;</p> 
<p><em>If you have questions, feedback, or would like to get involved in discussions with other community members, visit the&nbsp;<a href="https://forums.aws.amazon.com/category.jspa?categoryID=43" rel="noopener noreferrer" target="_blank">AWS Developer Forums: Media Services.</a></em></p>
