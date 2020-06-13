# OWASP ZAP CLI - generating PDF report using Export Report add-on and WkHTMLtoPDF

> This article is published also on [DEV Community](https://dev.to/c0d3b0t/owasp-zap-cli-generating-pdf-report-using-export-report-add-on-and-wkhtmltopdf-3i98/edit)

Hello community.

## Intro

In the last post I described the web application scanning with [Zaproxy CLI](https://www.zaproxy.org/docs/desktop/cmdline/) installed on [Kali Linux](https://www.kali.org/).

Zaproxy by default generates an output in XML format. It's cool, but I want a PDF report.

I've installed the [Export Report](https://www.zaproxy.org/docs/desktop/addons/export-report/) add-on that provides an ability to generate nice reports in different formats (`.xhtml`, `.pdf`, `.json`, `.xml`).

{% link https://dev.to/c0d3b0t/scanning-web-application-with-owasp-zap-3gkn %}

## Scan & Export

Now, let's put together a simple bash script.

```bash
#!/bin/bash

# Getting the host passed as an argument to the script
host="$1"

# Getting current timestamp to use it in the session name
timestamp=$(date '+%s');

# Exit if host is not specified
if [ -z "$host" ]; then
    echo -e "Please pass the host argument.\r"
    exit 1
fi

# Launching the scan
/usr/share/zaproxy/zap.sh -quickurl "$host" -newsession "$timestamp" -cmd;

# Defining variables that contain metadata for the report
report_name="Vulnerability Report - $host"
prepared_by="h4x0r"
prepared_for="X Corp"
scan_date=$(date -d @$timestamp)
report_date=$(date -d @$timestamp)
scan_version="N/A"
report_version="N/A"
report_description="Home page vulnerability report of the Example project."
file_name="$timestamp"

# Getting the report generated in XHTML format
/usr/share/zaproxy/zap.sh -export_report "$HOME"/"$file_name".xhtml -source_info "$report_title;$prepared_by;$prepared_for;$scan_date;$report_date;$scan_version;$report_version;$report_description" -alert_severity "t;t;f;t" -alert_details "t;t;t;t;t;t;f;f;f;f" -session "$timestamp.session" -cmd
```

[Here](https://www.zaproxy.org/docs/desktop/addons/export-report/) you can read about "Export Report" command line options.

## PDF is not supported

Unfortunately the 2.9.0 version I've installed, does not support output in `.pdf` format. There was an issue and PR for that, so  future releases probably will include that fix.

{% github https://github.com/zaproxy/zap-extensions/pull/2177 %}

At this point I don't want to upgrade anything, just because I want to have "Latest release" instead of "Pre-release", so I have a workaround - [WkHTMLtoPDF](https://wkhtmltopdf.org/).

## The Workaround

*If you're from the future - you're probably able to generate PDF report without this workaround.*

The idea is to get the report in XHTML format and convert it to PDF programmatically. 

### Installation

Because I'm using Kali, which is based on Debian, I need to download the `deb` package (check the [Downloads](https://wkhtmltopdf.org/downloads.html) section if you need something else).

`wget https://github.com/wkhtmltopdf/packaging/releases/download/0.12.6-rc/wkhtmltox_0.12.6-0.20200605.30.rc.faa06fa.stretch_amd64.deb`

Now install the package.

`sudo dpkg -i wkhtmltox_0.12.6-0.20200605.30.rc.faa06fa.stretch_amd64.deb`

### Usage

To convert `html/xhtml` to `pdf`, simply run a command:

`wkhtmltopdf file.xhtml file.pdf`

## Updating the bash script

Now let's convert the XHTML report to PDF using the `wkhtmltopdf` tool. We just need to include single line to the end of the script.

```bash
#!/bin/bash

# Get the host passed as an argument to the script
host="$1"

# Getting current timestamp to use it in the session name
timestamp=$(date '+%s');

# Exit if host is not specified
if [ -z "$host" ]; then
    echo -e "Please pass the host argument.\r"
    exit 1
fi

# Launching the scan
/usr/share/zaproxy/zap.sh -quickurl "$host" -newsession "$timestamp" -cmd;

# Defining variables that contain metadata for the report
report_name="Vulnerability Report - $host"
prepared_by="h4x0r"
prepared_for="X Corp"
scan_date=$(date -d @$timestamp)
report_date=$(date -d @$timestamp)
scan_version="N/A"
report_version="N/A"
report_description="Home page vulnerability report of the Example project."
file_name="$timestamp"

# Getting the report generated in XHTML format
/usr/share/zaproxy/zap.sh -export_report "$HOME"/"$file_name".xhtml -source_info "$report_title;$prepared_by;$prepared_for;$scan_date;$report_date;$scan_version;$report_version;$report_description" -alert_severity "t;t;f;t" -alert_details "t;t;t;t;t;t;f;f;f;f" -session "$timestamp.session" -cmd

# Converting XHTML report to PDF
wkhtmltopdf "$HOME"/"$file_name".xhtml "$HOME"/"$file_name".pdf
```

Hopefully my comments in the script are clear enough.

[Here is the Gist](https://gist.github.com/c0d3b0t/5139c61104f232206c49897701811b0d)

Though you can download it using `wget`.

`wget https://gist.githubusercontent.com/c0d3b0t/5139c61104f232206c49897701811b0d/raw/3de6356df81ed0a34310a095463e4c03ce2ee62a/run-zap.sh`

You can launch that script by running `bash run-zap.sh http://example.com` where "example.com" is your target.

The generated report looks like this.

![Export Report](https://dev-to-uploads.s3.amazonaws.com/i/xy7xey4z997cks714tuy.png)

So, now I've got same report in 2 different formats - XHTML and PDF. I've got XHTML exported by zap, and PDF converted using wkhtmltopdf.

Not bad... maybe.

## What's next?

I need to think more about the automation process I want to implement. The process I have currently is:

1) run the bash script
2) download the report using `scp` or FileZilla
