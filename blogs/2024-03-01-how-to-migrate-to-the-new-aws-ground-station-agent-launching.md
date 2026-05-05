---
title: "How to migrate to the new AWS Ground Station Agent launching March 28"
url: "https://aws.amazon.com/blogs/publicsector/how-to-migrate-to-the-new-aws-ground-station-agent-launching-march-28/"
date: "Fri, 01 Mar 2024 13:19:27 +0000"
author: "Oleg Grytsynevych"
feed_url: "https://aws.amazon.com/blogs/publicsector/tag/aws-ground-station/feed/"
---
<p><a href="https://d2908q01vomqb2.cloudfront.net/9e6a55b6b4563e652a23be9d623ca5055c356940/2024/02/29/ground_station_agent_header.png"><img alt="AWS branded background design with text overlay that says &quot;How to migrate to the new AWS Ground Station Agent launching March 28&quot;" class="aligncenter wp-image-21621 size-full" height="1024" src="https://d2908q01vomqb2.cloudfront.net/9e6a55b6b4563e652a23be9d623ca5055c356940/2024/02/29/ground_station_agent_header.png" width="2048" /></a></p> 
<p>On April 12, 2023, Amazon Web Services (AWS) announced <a href="https://aws.amazon.com/about-aws/whats-new/2023/04/aws-ground-station-wideband-digital-intermediate-frequency/">the general availability of Wideband Digital Intermediate Frequency (DigIF)</a> for satellite operators using Software Defined Radios (SDRs) with <a href="https://aws.amazon.com/ground-station/">AWS Ground Station</a>. Wideband DigIF data can be delivered to <a href="https://aws.amazon.com/s3/">Amazon Simple Storage Service</a> (Amazon S3) or to <a href="https://aws.amazon.com/ec2/">Amazon Elastic Compute Cloud</a> (Amazon EC2). Customers using Wideband DigIF delivery to Amazon EC2 need to provision <a href="https://docs.aws.amazon.com/ground-station/latest/ug/overview.html">AWS Ground Station Agent</a> (agent) on Amazon EC2. The agent enables secure, reliable, high rate and low jitter data delivery.</p> 
<p>On March 28, AWS will release a new version of the agent, which <strong>is not compatible with past agent releases</strong>. In order to maintain operational continuity of Ground Station environments, agent users <strong><u>must</u></strong> follow the instructions provided in this blog post before upgrading to the March 28 version of the agent.</p> 
<p><strong>Note: </strong>If your Amazon EC2 receiver instance is already configured to have an Elastic IP associated with a network interface—which is associated with a public subnet—and if the agent on the instance has CPU cores allocation corresponding to the new agent version (shown in Table 1), then there is no action needed. Please still consider reading this post to be aware of the changes.</p> 
<p>The new release of the agent introduces the following changes, which require actions from current users:</p> 
<ul> 
 <li>With past releases, the Amazon EC2 receiver instances (instances with provisioned agent) were allowed to have the Elastic IP associated with a network interface, which is associated with a private subnet. The new agent version requires that the Elastic IP is associated with a network interface associated with a public subnet (see the <em>Security aspects of agent operations</em> section of this post). If you already have your Elastic IP associated with a network interface associated with a public subnet, then no change is required for this step.</li> 
 <li>The new agent version requires additional CPU cores, shown in Table 1.</li> 
</ul> 
<h2>Migration to the new agent version</h2> 
<p>The migration path to the new version of the agent is shown in Figure 1 and is described in the text that follows.</p> 
<div class="wp-caption aligncenter" id="attachment_21617" style="width: 1175px;">
 <a href="https://d2908q01vomqb2.cloudfront.net/9e6a55b6b4563e652a23be9d623ca5055c356940/2024/02/29/ground_station_agent_figure1.png"><img alt="" class="size-full wp-image-21617" height="569" src="https://d2908q01vomqb2.cloudfront.net/9e6a55b6b4563e652a23be9d623ca5055c356940/2024/02/29/ground_station_agent_figure1.png" width="1165" /></a>
 <p class="wp-caption-text" id="caption-attachment-21617">Figure 1. A timeline of the release of the new version of the agent and the changes which customers must make.</p>
</div> 
<ul> 
 <li><strong>Do before</strong><strong> March</strong><strong> 13:</strong> In your AWS account, <strong>identify your existing Wideband DigIF stacks</strong> which you want to have operational after March 28, and collect <a href="https://aws.amazon.com/cloudformation/">AWS CloudFormation</a> templates for them. We call them customer “legacy” CloudFormation templates. The customer legacy CloudFormation templates are normally based on the Wideband DigIF CloudFormation template, released on April 12, 2023 (we call it the “legacy” Wideband DigIF CloudFormation template).</li> 
 <li><strong>Do before </strong><strong>March </strong><strong>13:</strong> <strong>F</strong><strong>etch the updated </strong><a href="https://docs.aws.amazon.com/ground-station/latest/ug/wb-digif-pbs.html"><strong>Wideband DigIF CloudFormation template</strong></a> provided by AWS. The CloudFormation template is compatible with the requirements of the new agent version. We will call it a “new” Wideband DigIF CloudFormation template.</li> 
 <li><strong>Do before March 13: Create a customer “test” CloudFormation template</strong> by adding your SDR-specific code into the new Wideband DigIF CloudFormation template, which makes sure the receiver Amazon EC2 instance has an elastic network interface (ENI) in a public subnet. Alternatively, if customers have highly customised Wideband DigIF CloudFormation templates, they may choose to modify their existing templates to make sure the receiver Amazon EC2 instance has an ENI in a public subnet. In addition, please increase the amount of cores allocated to the agent following the values in Table 1. You will need to edit parameter agentCpuCores of AGENT_CONFIG in the UserData. Please make sure you also reallocate CPU cores to SDR and other processes, so they don’t interfere with the agent cores (this might be reflected in the <em>Updated SDR &amp; customer-specific code</em> step in Figure 1).</li> 
</ul> 
<h3>Table 1. CPU core allocation for agent</h3> 
<table border="1" class="aligncenter"> 
 <tbody> 
  <tr> 
   <td rowspan="2"><strong>AntennaDownlink Bandwidth (MHz)</strong></td> 
   <td rowspan="2"><strong>Expected VITA-49.2 DigIF Datarate (MB/s)</strong></td> 
   <td colspan="2"><strong>Number of Cores (HT CPU Pairs)</strong></td> 
  </tr> 
  <tr> 
   <td>Agent Current Version</td> 
   <td>Agent &nbsp;New Version</td> 
  </tr> 
  <tr> 
   <td>50</td> 
   <td>1000</td> 
   <td>2</td> 
   <td>3</td> 
  </tr> 
  <tr> 
   <td>100</td> 
   <td>2000</td> 
   <td>3</td> 
   <td>4</td> 
  </tr> 
  <tr> 
   <td>150</td> 
   <td>3000</td> 
   <td>4</td> 
   <td>5</td> 
  </tr> 
  <tr> 
   <td>200</td> 
   <td>4000</td> 
   <td>4</td> 
   <td>6</td> 
  </tr> 
  <tr> 
   <td>250</td> 
   <td>5000</td> 
   <td>5</td> 
   <td>6</td> 
  </tr> 
  <tr> 
   <td>300</td> 
   <td>6000</td> 
   <td>5</td> 
   <td>7</td> 
  </tr> 
  <tr> 
   <td>350</td> 
   <td>7000</td> 
   <td>6</td> 
   <td>8</td> 
  </tr> 
  <tr> 
   <td>400</td> 
   <td>8000</td> 
   <td>7</td> 
   <td>9</td> 
  </tr> 
 </tbody> 
</table> 
<ul> 
 <li><strong>Do before March 13</strong>: <strong>Deploy test stacks</strong> based on your customer test CloudFormation template. The customer test CloudFormation template is based on the new Wideband DigIF template, which makes sure the receiver Amazon EC2 has an Elastic IP associated with a network interface associated with a public subnet. Values of CloudFormation template parameters SubnetId and PublicSubnetId determine how the Amazon EC2 instance will be deployed. The following Table 2 has two alternatives.</li> 
</ul> 
<h3>Table 2. Deployment options for the receiver Amazon EC2</h3> 
<table border="1"> 
 <tbody> 
  <tr> 
   <td><strong>SubnetId</strong></td> 
   <td><strong>PublicSubnetId</strong></td> 
   <td><strong>EC2 deployment</strong></td> 
  </tr> 
  <tr> 
   <td>&lt;public-subnet-id&gt;</td> 
   <td></td> 
   <td>The receiver EC2 is deployed into the public subnet &lt;public-subnet-id&gt;</td> 
  </tr> 
  <tr> 
   <td>&lt;private-nated-subnet-id&gt;</td> 
   <td>&lt;public-subnet-id&gt;</td> 
   <td>The EC2 instance is deployed into a private subnet &lt;private-nated-subnet-id&gt;, and the EC2 instance will have have a network interface associated with the &lt;public-subnet-id&gt;.</td> 
  </tr> 
 </tbody> 
</table> 
<p>The deployment (if done before March 28) will use the current (legacy) version of the agent and is intended to make sure that the new vCPU allocation for the agent and (potentially) new Amazon EC2 placement in the VPC work nominally.</p> 
<ul> 
 <li><strong>Do before </strong><strong>March </strong><strong>13</strong>: <strong>Schedule a test contact</strong> with the test workload deployed in the previous step, and make sure the contact works and the SDR receives and processes the satellite data as expected. Please contact your AWS account team in case the contact doesn’t work as expected.</li> 
 <li><strong>Do before </strong><strong>March </strong><strong>13</strong>: If you are satisfied with the preceding test results, please delete <strong>your </strong><strong>t</strong><strong>est workload</strong> (it’s based on the legacy agent and won’t work after March 28).</li> 
 <li><strong>Do before </strong><strong>March </strong><strong>13</strong>: Confirm with your AWS account team that you are ready to migrate to the new agent version.</li> 
 <li><strong>Do before </strong><strong>March </strong><strong>28</strong>: <strong>C</strong><strong>ancel your </strong><strong>AWS </strong><strong>G</strong><strong>round </strong><strong>S</strong><strong>tation</strong><strong> contacts scheduled for </strong><strong>March </strong><strong>28 and onwards </strong>with your legacy production workloads.</li> 
 <li><strong>On March </strong><strong>28</strong> (the new agent version is released): <strong>Deploy your “</strong><strong>n</strong><strong>ew </strong><strong>p</strong><strong>roduction” workload </strong>using the customer test CloudFormation template from the previous step. The deployment will fetch and use the new agent version (the new agent version will be available from March 28 on the same URL as the legacy agent).</li> 
 <li><strong>On March </strong><strong>28</strong>: <strong>S</strong><strong>chedule your post-</strong><strong>March </strong><strong>28 contacts</strong> with the mission profile, corresponding to the new production workload.</li> 
 <li><strong>On or after March </strong><strong>28</strong>: <strong>D</strong><strong>elete your </strong><strong>l</strong><strong>egacy production workloads</strong>.</li> 
 <li><strong>Final state</strong>: <strong>A</strong><strong>ll customer Wideband DigIF workloads</strong> are provisioned with the customer new production CloudFormation template and <strong>use the new agent version</strong>.</li> 
</ul> 
<p>Contact your AWS account team if you face any difficulties during the migration.</p> 
<h2>Security aspects of agent operations</h2> 
<p><a href="https://aws.amazon.com/blogs/enterprise-strategy/security-at-aws/">Security is job zero at AWS</a>. AWS employs the <a href="https://aws.amazon.com/compliance/shared-responsibility-model/">Shared Responsibility Model</a> where AWS is responsible for “Security of the Cloud” and customers are responsible for “Security in the Cloud.”</p> 
<p>The following diagrams show possible deployment options of Wideband DigIF workloads with the new agent (for brevity, the diagrams omit the <a href="https://aws.amazon.com/lambda/">AWS Lambda</a> starting and stopping the receiver Amazon EC2 instance). Figure 2 illustrates deployment into a public subnet.</p> 
<div class="wp-caption aligncenter" id="attachment_21619" style="width: 547px;">
 <a href="https://d2908q01vomqb2.cloudfront.net/9e6a55b6b4563e652a23be9d623ca5055c356940/2024/02/29/ground_station_agent_figure2.png"><img alt="" class="wp-image-21619 size-full" height="481" src="https://d2908q01vomqb2.cloudfront.net/9e6a55b6b4563e652a23be9d623ca5055c356940/2024/02/29/ground_station_agent_figure2.png" width="537" /></a>
 <p class="wp-caption-text" id="caption-attachment-21619">Figure 2. Architectural diagram of a receiver Amazon EC2 instance deployed into a public subnet. AWS Ground Station sends Wideband DigIf data to the Elastic IP, associated with an ENI associated with a public subnet.</p>
</div> 
<p>Figure 3 illustrates deployment into a network address translation (NAT-ed) private subnet.</p> 
<div class="wp-caption aligncenter" id="attachment_21620" style="width: 657px;">
 <a href="https://d2908q01vomqb2.cloudfront.net/9e6a55b6b4563e652a23be9d623ca5055c356940/2024/02/29/ground_station_agent_figure3.png"><img alt="" class="wp-image-21620 size-full" height="481" src="https://d2908q01vomqb2.cloudfront.net/9e6a55b6b4563e652a23be9d623ca5055c356940/2024/02/29/ground_station_agent_figure3.png" width="647" /></a>
 <p class="wp-caption-text" id="caption-attachment-21620">Figure 3. Architectural diagram of a receiver Amazon EC2 instance deployed into a private NAT-ed subnet. AWS Ground Station sends Wideband DigIf data to the<br />Elastic IP, associated with an ENI associated with a public subnet.</p>
</div> 
<p>The receiver Amazon EC2 (running the agent and SDR) along with the associated ENIs are customer-managed resources and therefore it is important that the customers employ the following security best practices to manage the resources:</p> 
<h3>Security of data in transit</h3> 
<ul> 
 <li>Data sent between AWS Ground Station and the agent is encrypted with a KMS key. See the <a href="https://docs.aws.amazon.com/ground-station/latest/ug/wb-digif-pbs.html">documentation about the Data Delivery Key</a>.</li> 
 <li>Customers accessing the control panel of SDR provisioned on the receiver have to contact the SDR vendor to clarify what secure types of communication the SDR offers for the control panel (for example, it can be HTTPS for web-based control panels).</li> 
 <li>Customers moving data decoded by the SDR outside Amazon EC2 should use secure channels, such as copying the decoded data as files to an Amazon S3 bucket via HTTPS. In cases of real-time streaming of the data out of SDR, the SDR vendor may guide the customer about the traffic encryption options, otherwise the customer may consider employing solutions like <a href="https://nmap.org/ncat/guide/ncat-ssl.html">SSL encryption for Ncat</a>.</li> 
</ul> 
<h3>Network security</h3> 
<ul> 
 <li>As the receiver Amazon EC2 has an ENI in a public subnet, it’s important to make sure the ENI receives only allowed traffic. To control the incoming traffic, customers use <a href="https://docs.aws.amazon.com/vpc/latest/userguide/vpc-security-groups.html">security groups</a>. The provided CloudFormation template (see InstanceSecurityGroup on line 221) allows UDP traffic to ports 42000-50000 from AWS Ground Station (limited to source addresses, which are part of the <a href="https://docs.aws.amazon.com/ground-station/latest/ug/best-practices.html#managed-prefix-list-best-practice">service prefix</a>), along with SSH traffic.</li> 
 <li>Instead of opening an SSH port on Amazon EC2, we recommend using <a href="https://docs.aws.amazon.com/systems-manager/latest/userguide/session-manager.html">AWS Systems Manager Session Manager (Session Manager)</a> as a more secure option. Session Manager allows users to remotely connect to the receiver Amazon EC2 instance using the Session Manager agent instead of SSH. Systems Manager Session Manager uses an always open connection from the agent to Systems Manager endpoints and does not require inbound rules on the security group attached to the instance.</li> 
 <li>The provisioned SDR may require opening more ports, and hence adding them to the InstanceSecurityGroup, for example to access the SDR’s control panel. We recommend customers consult with their SDR vendor to understand what ports are required to be opened.</li> 
 <li>The customers operating AWS Ground Station workloads at scale, who would like to manage security groups in a centralised way, may consider using <a href="https://aws.amazon.com/firewall-manager/">AWS Firewall Manager</a> or <a href="https://aws.amazon.com/config/">AWS Config</a>.</li> 
</ul> 
<h3>Security of data in rest</h3> 
<ul> 
 <li>The receiver Amazon EC2 has storage through <a href="https://aws.amazon.com/ebs/">Amazon Elastic Block Store</a> (Amazon EBS). Customers may consider <a href="https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EBSEncryption.html#encryption-by-default">Amazon EBS encryption by default</a> which applies at Region-level, or for more granular control at the CloudFormation template level for each deployed stack, <a href="https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-ec2-instance-ebs.html">as described in documentation</a>.</li> 
</ul> 
<h3>Software updates and vulnerability management</h3> 
<ul> 
 <li>The customer is responsible for regular updates of software provisioned on the receiver Amazon EC2, including OS components, the agent, and SDR.</li> 
 <li>Customers can update and patch their OS of the receiver Amazon EC2 instance with <a href="https://docs.aws.amazon.com/systems-manager/latest/userguide/run-command.html">Systems Manager Run Command</a>. using <a href="https://docs.aws.amazon.com/systems-manager/latest/userguide/patch-manager-aws-runpatchbaseline.html">AWS-RunPatchBaseline</a>. This document will not update the Ground Station Agent or SDR vendor software installed without the OS tools.</li> 
 <li>Customers can automate the update of the agent with <a href="https://docs.aws.amazon.com/systems-manager/latest/userguide/run-command.html">Systems Manager Run Command</a>. Customers <a href="https://docs.aws.amazon.com/systems-manager/latest/userguide/documents-creating-content.html">must create their own Systems Manager document </a>to specify to Systems Manager how to download, install, and reload the agent on the target agent instances.</li> 
</ul> 
<p style="padding-left: 40px;">The code snippet bellow contains an Systems Manager document example in JSON format to download the latest version of the agent, update the agent, and reload the agent after update.</p> 
<pre><code class="lang-json">{
  "schemaVersion": "2.2",
  "description": "Download and installs latest version of Ground Station Agent (GSA).",
  "mainSteps": [
    {
      "action": "aws:downloadContent",
      "name": "downloadGSA",
      "inputs": {
        "SourceType": "S3",
        "SourceInfo": {
          "path": "https://groundstation-wb-digif-software-us-east-2.s3-us-east-2.amazonaws.com/aws-groundstation-agent/latest/amazon_linux_2_x86_64/aws-groundstation-agent.rpm"
        },
        "destinationPath": "/tmp/"
      }
    },
    {
      "action": "aws:runShellScript",
      "name": "UpdateGSA",
      "inputs": {
        "timeoutSeconds": "3600",
        "runCommand": [
          "#!/bin/bash",
          "yum -y install /tmp/aws-groundstation-agent.rpm"
        ]
      }
    },
    {
      "action": "aws:runShellScript",
      "name": "GSAReload",
      "inputs": {
        "timeoutSeconds": "3600",
        "runCommand": [
          "#!/bin/bash",
          "systemctl restart aws-groundstation-agent",
          "systemctl status aws-groundstation-agent"
        ]
      }
    }
  ]
}</code></pre> 
<ul> 
 <li>Once you have created the document, please follow instructions to <a href="https://docs.aws.amazon.com/systems-manager/latest/userguide/running-commands-console.html">run the document through the Systems Manager Run Command.</a></li> 
 <li>We recommend customers seek guidance from their SDR vendors about the recommended ways of updating their SDR software.</li> 
 <li>We recommend customers establish a test environment, isolated from their production environment, for testing updates of OS, the agent, and SDR on the receiver Amazon EC2 instances. Please schedule a test contact with a satellite in the test environment after doing the update of the components to ensure everything operates nominally. Once the nominal operation in the test environment is confirmed, you can perform the respective updates in your production environment.</li> 
</ul> 
<h2>Conclusion</h2> 
<p>In this post, we described new requirements imposed by the new version of AWS Ground Station Agent which will be released March 28. We described a migration process from the legacy version of the agent to the new version. We encourage current users of the agent to follow the migration procedure and the security best practices covered in the post.</p>
