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
  - This will redirect the domain www\.khanhsplan.com to simply khanhsplan.com.

## Step 3. Secured website by utilizing SSL certificate with AWS Certificate Manager, ELB, and CloudFront CDN.

-
