---
title: "Quick and easy Media Services Dashboards"
url: "https://aws.amazon.com/blogs/media/cs-quick-and-easy-media-services-dashboards/"
date: "Mon, 26 Jul 2021 19:00:05 +0000"
author: "Rob Clements"
feed_url: "https://aws.amazon.com/blogs/media/tag/aws-elemental-mediaconnect/feed/"
---
<p>New users to the <a href="https://aws.amazon.com/media-services/">AWS Media Services</a> often find the wide assortment of metrics available to be confusing. Users often ask which metrics are most meaningful, how they can make the metrics easily viewed in a chart, and how to view metrics from multiple services on a single Dashboard.</p> 
<p>Additionally, customers new to AWS may not have the time or expertise to build useful <a href="https://aws.amazon.com/cloudwatch/">Amazon CloudWatch</a> Dashboards. To help customers better monitor their Media Service workflows, we created a set of fast and simple self-service scripts that make it easy to generate CloudWatch Dashboards. These Dashboards provide reliable confidence monitoring and error intelligence for critical real-time content.&nbsp; These self-contained, self-service scripts allow users to easily create a new Dashboard for the relevant services in approximately 15 seconds.</p> 
<p>There is a second audience for these quick setup tools: Experienced users of AWS Media Services who are on short deadlines for a special live event, and require event-specific Dashboards to track multiple services participating in this upcoming event.</p> 
<h2>Self-service scripts for on-demand Dashboard creation</h2> 
<p>This new collection of scripts work by querying the desired service using the <a href="https://aws.amazon.com/cli/">AWS Command Line Interface</a> (AWS CLI) or <a href="https://aws.amazon.com/cloudshell/">AWS CloudShell</a> prompt, asking for optional name filters, then assembling a new CloudWatch Dashboard using the top four or five most useful metrics for a given service (as defined by the AWS platform engineering teams). Users can quickly invoke these scripts to create useful Dashboards, and then optionally customize them from the CloudWatch Console as needed.</p> 
<p>Users can optionally focus each Dashboard on just one resource (such as a single <a href="https://aws.amazon.com/mediapackage/">AWS Elemental MediaPackage</a> Channel),&nbsp; OR users can tag a subset of their resources (for example, only the two <a href="https://aws.amazon.com/medialive/">AWS Elemental MediaLive</a> inputs and the correlating MediaPackage channel being used for a live stream event).&nbsp; This process requires the user to tag the resources individually using the AWS Management Console or the AWS CLI.&nbsp; Then, when you run the script, select the “[t]agged” option to create a Dashboard with only the tagged resources.</p> 
<p>The included “Dashboard Joiner” script allows users to aggregate service-specific widgets into a multi-service Dashboard representing each step in a multi-service workflow.</p> 
<p>The following is a typical result. This 3-service Dashboard with 11 widgets took less than a minute to create using these scripts.</p> 
<div class="wp-caption aligncenter" id="attachment_8899" style="width: 1076px;">
 <img alt="Image of Dashboard with widgets from multiple AWS services " class="wp-image-8899 size-full" height="433" src="https://d2908q01vomqb2.cloudfront.net/fb644351560d8296fe6da332236b1f8d61b2828a/2021/07/23/Picture1-5.png" width="1066" />
 <p class="wp-caption-text" id="caption-attachment-8899"><em>Image of Dashboard with widgets from multiple AWS services</em></p>
</div> 
<h2>Using the scripts</h2> 
<p><strong>Pre-Requisites</strong></p> 
<ol> 
 <li>The scripts require access to the AWS CLI to poll the service details. If you do not have access to a local computer with AWS CLI installed, you can also run these scripts from the AWS CloudShell prompt within your AWS Management Console.</li> 
 <li>The scripts need be in your local directory. You download a zip file containing all the scripts from<a href="https://ibdfq4kkcklucf.data.mediastore.us-west-2.amazonaws.com/CW_Scripts/All_Latest_Scripts_060621.zip"> this link</a>, or from a CloudShell/AWS CLI prompt using a command of the form:<code>wget &nbsp;<a href="https://dlcc9uvp8v2in.cloudfront.net/LATEST.zip">https://dlcc9uvp8v2in.cloudfront.net/LATEST.zip</a>&nbsp;&amp;&amp; unzip *.zip</code></li> 
</ol> 
<p><strong>Using the Scripts</strong></p> 
<p>To make a new Dashboard for AWS Elemental MediaLive inputs:</p> 
<ol> 
 <li>Run the command &nbsp;python3 EML_Inputs_dash_maker_v12</li> 
 <li>The script prompts you to choose to include [a]ll channels, only [t]agged channels, or a [s]ingle channel. Press “<strong>a</strong>”, “<strong>t</strong>” or “<strong>s</strong>” according to your desired result.</li> 
 <li><strong>Enter</strong> any secondary information per the prompts.</li> 
 <li>The script generates a JSON file and then uploads it to create the new CloudWatch Dashboard. The JSON file mentioned previously is saved locally, so that you can optionally join several JSON files together to create a multi-service Dashboard.</li> 
 <li>(Optional) Run the command ‘python3 DashJoin_v3.py’ to join several Dashboard files and create a new Dashboard containing the combined metrics.</li> 
</ol> 
<p>Note on tagging resources:&nbsp; most AWS services allow you to tag individual resources (an input, or a channel) from the AWS Management Console. Other services (including MediaPackage) require a CLI command to tag a resource. &nbsp;Refer to the ‘<strong>tag-resource’ </strong>sub-command within the service of interest for more details.</p> 
<h3>Next Steps</h3> 
<ul> 
 <li>You may further customize the Dashboards using the CloudWatch console, to change timeframes, colors, or other chart properties. Dashboards may also be exported, renamed, and deleted from the CloudWatch console.</li> 
 <li>You may optionally set alarm thresholds for each widget, so that when a value drops below a safe threshold, you are automatically notified. See <a href="https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/AlarmThatSendsEmail.html">Using Amazon CloudWatch alarms</a> for more information.</li> 
</ul> 
<h2>Conclusion</h2> 
<p>These simple scripts can easily and rapidly create useful monitoring Dashboards for a variety of user workflows leveraging the AWS Elemental media services. These tools provide insight, confidence monitoring and diagnostic help when troubleshooting complex workflows. AWS encourages customers to explore Amazon CloudWatch Dashboards as a means of efficiently and proactively monitoring service health and performance</p> 
<p>&nbsp;</p> 
<p class="p1"><span class="s1">If you have questions, feedback, or would like to get involved in discussions with other community members, visit the <a href="https://forums.aws.amazon.com/category.jspa?categoryID=43">AWS Developer Forums: Media Services</a>.</span></p>
