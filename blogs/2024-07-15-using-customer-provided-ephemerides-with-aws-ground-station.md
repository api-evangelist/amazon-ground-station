---
title: "Using customer-provided ephemerides with AWS Ground Station"
url: "https://aws.amazon.com/blogs/publicsector/using-customer-provided-ephemerides-with-aws-ground-station/"
date: "Mon, 15 Jul 2024 18:49:07 +0000"
author: "Viktor Pankov"
feed_url: "https://aws.amazon.com/blogs/publicsector/tag/aws-ground-station/feed/"
---
<p><img alt="AWS branded background design with text overlay that says &quot;Using customer-provided ephemerides with AWS Ground Station&quot;" class="aligncenter size-full wp-image-22878" height="1024" src="https://d2908q01vomqb2.cloudfront.net/9e6a55b6b4563e652a23be9d623ca5055c356940/2024/07/12/ephmerides_header.png" width="2048" /></p> 
<p>Amazon Web Services (<a href="https://aws.amazon.com/ground-station/">AWS) Ground Station</a> is a cloud-based service that provides you with an opportunity to perform communication sessions with your satellite without spending a fortune on your own&nbsp;ground station infrastructure. AWS Ground Station balances between providing a ready-made solution and tailoring the service to meet the unique needs of each customer. One of the ways to customize the service is to <a href="https://docs.aws.amazon.com/ground-station/latest/ug/providing-custom-ephemeris-data.html">use customer-provided ephemerides (CPE)</a>&nbsp;for antenna targeting. Ephemerides is the plural form of ephemeris, which are tabular representations detailing the positions and velocities of celestial objects,&nbsp;particularly satellites, at regular intervals, such as minute by minute.</p> 
<p><a href="https://docs.aws.amazon.com/ground-station/latest/ug/default-ephemeris-data.html">By default</a>, AWS Ground Station uses the orbital parameters downloaded from <a href="http://space-track.org/">space-track.org</a>.&nbsp;However, this solution is not applicable during the Launch and Early Orbit Phase (LEOP) or orbital&nbsp;maneuvers when ephemeris data is not yet accessible from <a href="https://www.norad.mil/">North American Aerospace Defense Command</a> (NORAD). Moreover, not all satellites are tracked by NORAD.</p> 
<p>This post gives an overview of the solution that provides customers with the functionality to upload CPE and&nbsp;update antenna-pointing instructions within the service. If you are ready to try it yourself, you can find the code at the <a href="https://github.com/aws-samples/aws-groundstation-cpe">aws-groundstation-cpe code repository</a>.</p> 
<h2>Prerequisites</h2> 
<p>The ephemeris API is currently in a Preview state and access is provided only on an as-needed basis. To request access, please email <a href="mailto:aws-groundstation@amazon.com">aws-groundstation@amazon.com</a>.</p> 
<p>Before starting this hands-on demo, <a href="https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/cfn-console-create-stack.html">learn how to create an AWS CloudFormation stack</a> or refresh your knowledge.</p> 
<p>To deploy the described pipeline you will need:</p> 
<ol> 
 <li>Access to an AWS account with sufficient permissions to use AWS Ground Station;</li> 
 <li>The guide in the repository uses the JPSS-1 craft as an example, so you must have it onboarded into your AWS account. Alternatively, you can use your own craft by setting the “SatelliteName” parameter in the CloudFormation template to your craft name.</li> 
 <li>To get JPSS-1 onboarded to your account for the CPE public beta, email&nbsp;<a href="mailto:aws-groundstation@amazon.com">aws-groundstation@amazon.com</a>&nbsp;with the following details:</li> 
</ol> 
<ul> 
 <li> 
  <ul> 
   <li>Satellite NORAD ID: 43013 (JPSS-1);</li> 
   <li>You AWS Account Id;</li> 
   <li>AWS Regions you want use the AWS Ground Station Service;</li> 
   <li>AWS Regions you want to downlink the data to, normally the same as above;</li> 
  </ul> </li> 
</ul> 
<h2>Ephemerides and input formats</h2> 
<p>Ephemerides play a crucial role in astronomy, navigation, and related fields. Ground stations rely on ephemerides to compute communication sessions, defining periods when a satellite is within the radio visibility zone of the antenna. Additionally, ephemerides are essential for accurately aligning antennas with satellites. The presented solution uses two main file formats for ephemerides: TLE and OEM.</p> 
<p><a href="https://en.wikipedia.org/wiki/Two-line_element_set">The two-line element set (TLE)</a> is a widely used format for representing orbital elements of Earth-orbiting objects, such as satellites. TLE consists of two lines of text with specific information about the satellite’s orbit. TLE provides a compact and standardized way of describing the position and motion of satellites in Earth’s orbit. It is commonly used in satellite tracking and by satellite prediction software. This&nbsp;format was defined by NORAD. Orbital elements are determined for many thousands of space objects from the NORAD database and are freely distributed for further use.</p> 
<p><a href="https://public.ccsds.org/Pubs/505x0b3e1.pdf" rel="noopener" target="_blank">The Orbit Ephemeris Message (OEM)</a> is one of the three standard file formats defined by the <a href="https://public.ccsds.org/default.aspx">Consultative Committee for Space Data Systems</a> (CCSDS) for transferring spacecraft orbit information. It is utilized in mission planning, spacecraft analysis, and simulation tools within the aerospace industry. The OEM format allows for a more comprehensive representation of orbital data compared to the simplified TLE format.</p> 
<p>In the following overview, we use TLE data for the&nbsp;<a href="https://directreadout.sci.gsfc.nasa.gov/?id=dspContent&amp;cid=246">NOAA-20 (JPSS-1) satellite</a>.&nbsp;It orbits the Earth in a Sun-synchronous near-polar orbit at an altitude of 825 kilometers, which makes it a low Earth orbit (LEO) satellite. You can also&nbsp;<a href="https://github.com/aws-samples/aws-groundstation-cpe#:~:text=To%20get%20JPSS,same%20as%20above">get JPSS-1 onboarded to your account for the CPE public beta</a>.</p> 
<h2>Solution overview</h2> 
<p>The presented solution creates a pipeline that uses AWS Ground Station CPE to update antenna-pointing instructions within the service. First, the data file (TLE or OEM) is uploaded to an <a href="https://aws.amazon.com/s3/">Amazon Simple Storage Service (Amazon S3)</a> bucket. The Amazon S3 bucket uses an <a href="https://aws.amazon.com/kms/">AWS Key Management Service (AWS KMS)</a> key for encryption. The upload triggers an <a href="https://aws.amazon.com/lambda/">AWS Lambda</a> function that updates ephemeris data for the configured satellite (steps 1–3&nbsp;of the high-level solution diagram in Figure 1).</p> 
<p>Once the CPE status shifts to ENABLED, the updated ephemeris can be used for antenna pointing. The Lambda function writes logs to the <a href="https://aws.amazon.com/cloudwatch/">Amazon CloudWatch</a> log group, facilitating troubleshooting if needed. The customer will be notified about CPE status updates by SNS topic (steps 4–5 in Figure 1).</p> 
<p>It is important to note that already scheduled communication sessions will be performed with the previous ephemeris. In order to perform automatic contact rescheduling in the pipeline, there is an additional&nbsp;Lambda function triggered by the update of the CPE entry transitioning to the ENABLED state.&nbsp;The function cancels all scheduled sessions within the next six days&nbsp;for a specified mission profile and satellite. Subsequently, it reschedules the closest available communication sessions using the updated ephemeris (steps 6–8 in Figure 1).</p> 
<div class="wp-caption aligncenter" id="attachment_22879" style="width: 931px;">
 <img alt="" class="size-full wp-image-22879" height="811" src="https://d2908q01vomqb2.cloudfront.net/9e6a55b6b4563e652a23be9d623ca5055c356940/2024/07/12/ephemerides_figure1.jpg" width="921" />
 <p class="wp-caption-text" id="caption-attachment-22879">Figure 1. System architecture diagram of the solution described in this post.</p>
</div> 
<h2>Implementation</h2> 
<p><a href="https://github.com/aws-samples/aws-groundstation-cpe#using-customer-provided-ephemeris-cpe-with-aws-ground-station">Follow the instructions in this code repository</a> to implement this sample workflow in your own AWS account.</p> 
<h2>Summary</h2> 
<p>The presented solution expands the usability of the service for situations where standard methods of obtaining ephemerides are not available. Now, you can use the service at the earliest stages of a space mission, as well as during maneuvers and other instances of orbit deviation from the forecast.</p> 
<p>Learn more about&nbsp;<a href="https://aws.amazon.com/ground-station/getting-started/">onboarding your satellite</a>&nbsp;to AWS Ground Station.</p>
