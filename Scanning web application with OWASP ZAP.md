# Scanning web application with OWASP ZAP

> This article is published also on [DEV Community](https://dev.to/c0d3b0t/scanning-web-application-with-owasp-zap-3gkn)

Hi there!

Let's try and have some scans running with [OWASP ZAP](https://owasp.org/www-project-zap/) :zap:.

## Connection

I'm running Kali on AWS so I want to connect to the instance using SSH.

I have the `.pem` file, so I need to run just few commands.

```bash
sudo chmod 400 kali.pem
ssh -i kali.pem ec2-user@your-public-dns
```

For Windows users there is a good article - [Connecting to your Linux instance from Windows using PuTTY](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/putty.html)

## Installation
I expected to have `zaproxy` preinstalled, but no. So, let's install it. Though I've installed the 2019.4 version of Kali.

Let's run the command and get the `zaproxy` installed: 

`sudo apt-get update && sudo apt-get install zaproxy`

Hopefully you've completed the installation successfully.

If you run the command `zaproxy`, you should probably see output like this:

```
Found Java version 11.0.5
Available memory: 982 MB
Using JVM args: -Xmx245m
0 [main] INFO org.zaproxy.zap.GuiBootstrap  - OWASP ZAP 2.9.0 started 30/05/2020, 14:57:21 with home /home/ec2-user/.ZAP/
2 [main] FATAL org.zaproxy.zap.GuiBootstrap  - ZAP GUI is not supported on a headless environment.
Run ZAP inline or in daemon mode, use -help command line argument for more details.
ZAP GUI is not supported on a headless environment.
Run ZAP inline or in daemon mode, use -help command line argument for more details.
```

We're using zap on a headless environment, so let's figure out how to use this tool in command line.

For some reason `zaproxy -cmd -help` command didn't work for me, so I had to figure out another way to run the tool.

The `whereis zaproxy` command shows us the following output `zaproxy: /usr/bin/zaproxy /usr/share/zaproxy`. 

We're looking for `zap.sh` file located at `/usr/share/zaproxy` directory. Windows users should look for `zap.bat` file.

![/usr/share/zaproxy directory](https://dev-to-uploads.s3.amazonaws.com/i/2eth2y0biq6337w1c4ic.png)

You can simply run it with `bash /usr/share/zaproxy/zap.sh` command.

### Making a globally available command `zap`

If you're too lazy to type as many characters, then you can make an alias `zap` to `/usr/share/zaproxy/zap.sh`
To do that, we need to perform few simple steps and edit the `.bashrc` file.

- Open the `.bashrc` file using vim or nano - `nano ~/.bashrc`
- Add the following code to the end of file - `alias zap="bash /usr/share/zaproxy/zap.sh"`
- Save the file and quit
- Run `source ~/.bashrc` to apply changes, otherwise you need to log out and log in again
- Run `zap -help` or `zap -version`

![zap help command](https://dev-to-uploads.s3.amazonaws.com/i/l9iyuf812odhooz0vqau.png)

As you can see I'm using version 2.9.0.

If your output is similar to mine, then we're done here! :rocket:

## Scan

Now we are ready to execute our first scan. Simply, run the following command:

`zap -cmd -quickurl http://example.com -quickprogress -quickout ~/out.xml`

Replace the "example.com" with whatever host you want to scan.

Here is my console output:
```
ec2-user@kali:~$ zap -cmd -quickurl http://example.com -quickprogress -quickout ~/out.xml
Found Java version 11.0.5
Available memory: 982 MB
Using JVM args: -Xmx245m
Accessing URL
Using traditional spider
Active scanning
[====================] 100% 
Attack complete
Writing results to /home/ec2-user/out.xml
```

So, we just ran an attack on `example.com` host and got the output in XML format - the out.xml file located in `/home/ec2-user` directory.

Good start. But there is a one problem - I don't want output to be in XML format. I want PDF! 

## Addons

There are lot of useful addons in the [ZAP Marketplace](https://www.zaproxy.org/addons/). We need the one named "[Export Report](https://www.zaproxy.org/docs/desktop/addons/export-report/)".

ZAP allows us to install addons by their ID. Let's install the addon:

`zap -cmd -addoninstall exportreport`

## What's next?

In the next post I want to figure out the usage of Export Report addon. 

In the end I want to have scheduled scans running automatically and generating me nice PDF reports.

Have a great day! :sunny:
