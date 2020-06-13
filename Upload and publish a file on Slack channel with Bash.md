# Upload and publish a file on Slack channel with Bash

> This article is published also on [DEV Community](https://dev.to/c0d3b0t/upload-and-publish-a-file-on-slack-channel-with-bash-i2e)

In my [previous post](https://dev.to/c0d3b0t/owasp-zap-cli-generating-pdf-report-using-export-report-add-on-and-wkhtmltopdf-3i98) I talked about exporting and generating PDF report from Zaproxy output.

Now I want to have my bash script updated to share that report to my Slack channel.

## Publish a file on Slack

### The Slack app

* Go to [api.slack.com](https://api.slack.com/).
* Click the ["Your Apps"](https://api.slack.com/apps) link at the top right.
* Authenticate, if you're not authenticated.
* Click the "Create an App" button.
![Slack Create App](https://dev-to-uploads.s3.amazonaws.com/i/4o3dz2gx36dl569idp8p.png)

* Set the App name, e.g. "Notifier", set the "Development Slack Workspace" and click the "Create App" button.
![Slack Create App Dialog](https://dev-to-uploads.s3.amazonaws.com/i/2zktfiw71yfvabjzt5dq.png)

### Scopes and Permissions

* Click the "OAuth & Permissions" menu item in the "Features" section, or the "Permissions" item in the "Add features and functionality" section. 
Scroll down to the "Scopes" section and under the "Bot Token Scopes", click the "Add an OAuth Scope" button. 
In the opened dropdown search and select the "files:write" permission.
![Slack Scopes](https://dev-to-uploads.s3.amazonaws.com/i/7omkmuas2h2yvcllha0b.png)

In case if you're adding a scope to an existing Slack app that is installed to workspace (e.g. you may already have a Slack app for incoming webhooks), a big yellow alert may ask you to reinstall the app to apply changes. Click the "reinstall your app" link, specify a Slack channel, and click the "Allow" button.
![Slack alert](https://dev-to-uploads.s3.amazonaws.com/i/j0johsx1frsa0tvfgux3.png)

* Scroll up and click the "Install App to Workspace" button.

You can see what permissions are given to the app. Because I've added the "files:write" scope, I'm allowing the app to upload, edit and delete files.
![Slack App Install to Workspace](https://dev-to-uploads.s3.amazonaws.com/i/0lguy0b38puglk55ltbh.png)

Click the "Allow" button.

* Copy and save the "Bot User OAuth Access Token".
![Slack OAuth & Permissions](https://dev-to-uploads.s3.amazonaws.com/i/8mckbm3saaw9855r7awx.png)

### Add the app to a channel

* Open the Slack app. In the Apps section click the bot name we've created. In the "Details" section, click the "More" button, then click the "Add this app to a channel...". Choose a channel and click the "Add" button.
![Slack add App to a channel](https://dev-to-uploads.s3.amazonaws.com/i/zhbt53mah81xndv8xss3.png)

### File uploading via API
* Before doing a call to the Slack API, we need to grab the ID of the channel we want to use to receive files. I couldn't figure out a better way to grab the channel ID, so if you know a better way - please share me a comment about that.  
I simply used the web interface of the Slack app. URL contains a channel ID. Just choose the channel you want to use and grab the ID from URL (the last segment). It's something like `G1258QGKLMN`.
![Slack Channel ID](https://dev-to-uploads.s3.amazonaws.com/i/ue2lyysx52hh1bg2bzo1.png)

* To test the file upload via slack API, we're going to use the `files.upload` endpoint. I'm using "curl" to do the call:
```bash
curl -F file=@example.txt -F "initial_comment=Example file" -F channels=<CHANNEL_ID>, <ANOTHER_CHANNEL_ID_IF_NEEDED> -H "Authorization: Bearer <BOT_USER_OAUTH_ACCESS_TOKEN>" https://slack.com/api/files.upload
``` 
It makes an attempt to upload the "example.txt" file to specified channels, and will add the "Example file" string to the actual slack message.
![Slack App Message](https://dev-to-uploads.s3.amazonaws.com/i/xl11j0rnp9d265tz5npn.png)


## Updating the `run-zap.sh` bash script from my previous posts

I just need to add an appropriate curl call to the bash script, and it should be enough to have the report shared to my Slack channel.

Here is the updated gist:

{% gist https://gist.github.com/c0d3b0t/5139c61104f232206c49897701811b0d %}

If you'll use that script and will need to have slack notifications working, do not forget to replace placeholders with real channel ID and Bot User OAuth Access Token as a Bearer token. If you don't need slack notification - simply comment out that line.

The usage of the script is simple:  
```bash
bash run-zap.sh http://example.org h4x0r X-Corp TopSecret
```
So, the `http://example.org` parameter is the target host, "h4x0r" is the person/organization who prepared the report, and "X-Corp" is the one for whom the report has been prepared. And of course, the "TopSecret" is the project name. These extra parameters are used for the metadata of the report.

