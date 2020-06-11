# Installing chromedriver and Selenium Server on Linux Mint 19.3 (Tricia)

> This article is published also on [DEV Community](https://dev.to/c0d3b0t/installing-chromedriver-and-selenium-server-on-linux-mint-19-3-tricia-5gnk)

## Download and install the chromedriver

First step is to check the Chrome browser version installed on your machine. Click the kebab menu icon at top right. Go to Help -> About Google Chrome. It shows I'm using `Version 81.0.4044.138 (Official Build) (64-bit)` so I have to download  [ChromeDriver 81.0.4044.138](https://chromedriver.storage.googleapis.com/index.html?path=81.0.4044.138/).

Go to [Chromedriver Downloads](https://chromedriver.chromium.org/downloads) page. Click the download link for appropriate version (both chromedriver and Chrome browser should have same version installed), click the `chromedriver_linux64.zip` archive to download it.

After the download is completed - extract and move the chromedriver to /usr/bin directory and make it executable.

```bash
sudo unzip chromedriver_linux64.zip -d /usr/bin
# make sure it is executable
sudo chmod +x /usr/bin/chromedriver
```

Now I can enter `chromedriver` command and have it running in my terminal.

## Download and install Selenium Server standalone

Go to [Selenium Downloads](https://www.selenium.dev/downloads/) page and download Selenium Server latest stable version. In my case it's 3.141.59.

The file I've got is a `.jar` file - `selenium-server-standalone-3.141.59.jar`. I just need to execute that file to have the server up and running.

```bash
java -jar selenium-server-standalone-3.141.59.jar
```

Now I'm able to run my acceptance tests with WebDriver enabled. Though, I'm running them using [Codeception](https://codeception.com/) framework.

## Bonus

In case if you're getting an error "Address already in use", find and kill the process launched by "java -jar selenium-server-standalone-3.141.59.jar" command.

```bash
ps aux | grep java
kill -9 <PID>
``` 



