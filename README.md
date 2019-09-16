# khanhsplan.com - Personal HTTPS WordPress blog using EC2, AMI, Route 53, CloudFront, and SSL certificate.

## Step 1. Install Wordpress with EC2 AMI.

- Go into AWS and select EC2 and "Launch Instance."
- You then want to go into the AWS Marketplace and search for "wordpress" and select "WordPress Certified by Bitnami and Automattic".
- Choose the t2.micro for your instanct type, choose your VPC Network or default and subnets, and be sure to select "Enable" for Auto-assign Public IP.
- Keep selecting Next until you are at the "Step 6: Configure Secturity Group". Be sure to allow HTTP (80), HTTPS (443), and SSH (22) when configuring your security group.
- Now you can Launch the instance. This will now create your EC2 instance.

## Step 2. Generate Domain name with Route 53 and redirect WordPress public IP to Domain name.

- You can register your domain name for $12/year here through Route 53 in AWS. I chose khanhsplan.com and created the domain name.
- You can then click on "Hosted zones" and select your new domain name.
- You now want to create a Record Set with Type A. You will then input your public IP that you get from the EC2 instance from Step 1 to the "Value" box. Save Record Set.
- Next, you will create another Record Set but this time you want to create a CNAME type and put 'www' in front of the domaine "Name". For "Value" you will input your domain name (without the www).
  - This will redirect the domain ww<span>w.kh</span>anhsplan.com to simply khanhsplan.com.

## Step 3. Secured website by utilizing SSL certificate with AWS Certificate Manager, ELB, and CloudFront CDN.

- Go into AWS Certificate Manager and select "Request a Certificate".
- Add the Domain names. I added 'ww<span>w.kh</span>anhsplan.com', 'khanhsplan.com', and '\*.khanhsplan.com'.
- You can then choose the "Email validation" to select a validation method. Then confirm and request. A validation email will be sent to wherever you set up the domain in Step 2.
- Once you approve from the email, your Validation status turned to "Success". You can now see your SSL/TLS certificate in "Certificate Manager".
- Now go to EC2 for Load Balancer and Create Load Balancer. Choose Application Load Balancer.
  - Choose a name, be sure it is "internet-facing" and IPv4. For your Listener Load Balancer Protocol, choose HTTP (80).
    - As precaution, I also added the HTTPS (443) Listener to the Load Balancer.
  - For "Certificate type", select "Choose a certificate from ACM (recommended)" and select the certificate that you created earlier in this Step.
  - Select your Security Group that exposes HTTP and HTTPS.
  - Select a New Target Group and input a name and you can leave the default protocols to HTTP (80) and choose "Instance" for target type and your VPC.
- Now go into the CloudFront service and create a web distribution.
  - Select your Load Balancer for your "Origin Domain Name". Also changed "Origin Protocol Policy" option to "Match Viewer".
  - Change "Viewer Protocol Policy" to "Redirect HTTP to HTTPS" under Default Cache Behavior Settings.
  - Input all 3 domain names under "Alternate Domain Names (CNAMEs) and change "SSL Certificate" to "Custom SSL Certificate (example.com)" and select your SSL Certificate from Certificate Manager.
  - Now Click "Create Distribution" to finish this step. This step will take awhile before the distribution becomes completed.
    - For a Dev environment, you can reduce the CloudFront TTL to a lower amount to see the changes. This helps as the default is 24 hours.
    - Once everything is secured, you can increase the TTL to the default or higher cache time.
- Return to Route 53 and update the A record sets with the CloudFront domain name
  - Update both ww<span>w.kh</span>anhsplan.com and khanhsplan.com A record to match the CloudFront domain name, using the CNAME and A type respectively.
  - You may not know when if this works completely until the next day. I would monitor and adjust accordingly as needed.
