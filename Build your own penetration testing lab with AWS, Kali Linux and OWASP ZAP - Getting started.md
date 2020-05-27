> This article is published also on [DEV Community](https://dev.to/c0d3b0t/build-your-own-penetration-testing-lab-with-aws-kali-linux-and-owasp-zap-getting-started-4083)

> Build your own penetration testing lab with AWS... or spend ton of money on various ~~expensive~~ scan services.

Hello everyone! 
I've decided to refuse security scan services and build a simple pentesting lab based on [Kali Linux](https://www.kali.org/).  

If you don't have an AWS account - it's the right time to [create](https://portal.aws.amazon.com/billing/signup?nc2=h_ct&src=header_signup&redirect_url=https%3A%2F%2Faws.amazon.com%2Fregistration-confirmation#/start) one!

## EC2 and Kali Linux

#### Few words...  

EC2 stands for Elastic Compute Cloud. In our case we'll use EC2 instance to get our Kali image installed, up and running.

Kali Linux is a Linux distribution based on [Debian](https://www.debian.org/). It's commonly designed for penetration testing purposes, and has a lot of useful tools preinstalled.

### Installation steps

- Login to your AWS account.
- On the home page, either search or select the EC2 service that is located under Compute category.
- On the EC2 Dashboard page, click the "Launch instance" instance button.
- We're looking for Kali Linux image, so, on the left sidebar, choose "AWS Marketplace", then use the search input field at the top - enter "Kali Linux" and hit the Enter/Return key on your keyboard.
- You should see the "Kali Linux" image in the search results.
![AWS Kali Linux image](https://dev-to-uploads.s3.amazonaws.com/i/76b065glpcbjit7vaj34.png)
- Click appropriate "Select" button.
- In the opened modal you see the description of Kali Linux and Pricing Details. Click "Continue".
- At this moment I don't care about best experience, so I choose "t2.micro" just because free tier eligible. Choose the instance type and click "Review and Launch". Clicking the "Review and Launch" button will redirect us to the last step. AWS has some default configuration setup for storage, tags, security groups etc. You can edit those configs if you want.  
- On "Step 7: Review Instance Launch" page you can review your instance configuration. Please make sure your security group allows SSH. 
![EC2 Security Groups](https://dev-to-uploads.s3.amazonaws.com/i/sek6cs7ynxd2d6tgzb5a.png)
- Click the "Launch" button.
- In the opened modal select the "Create a new key pair" option in the dropdown field, enter a key pair name (e.g. kalilinux), and click the "Download Key Pair" button.
![Alt Text](https://dev-to-uploads.s3.amazonaws.com/i/i6s3i3zd0f2ka401w5xx.png)
- Please make sure you have downloaded the key pair file, and click the "Launch instance" button.

That's all! Few moments later you should see new running ec2 instance - just click the "Instances" link in left menu.


## What's next?

In the next post I want to explore the power of OWASP ZAP and investigate to see how can I setup and automate some scans to prevent vulnerabilities. And of course, with beautiful reports and notifications...

If you have an idea that could help me in my research, please share in the comments. Thanks! :) 
