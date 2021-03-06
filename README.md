# qualyswas-notification-to-slack

## Abstract
Fetch a qualys WAS notification email, parses it and send a notification to Slack. Send a red notification if vulnerabilities above a certain threshold are discovered, green otherwise. Also red notification if the scan is not successful.

The email should be send to this script by piping (stdin). This can easily be done with your MTP server (exim, postfix...) by adding a special address to your mailserver. Edit the file `/etc/aliases` and add this line :
```
qualys: "|/path/to/custom-bash.sh"
```

The file custom-bash.sh will contain :
```
#!/bin/bash
export PYTHONPATH=/usr/local/lib/python3.4/site-packages
cat /dev/stdin | /path/to/qualyswas-notification-to-slack.py -S urgent -U 'https://hooks.slack.com/services/XXXXXX/XXXXXXX/XXXXXXXXXXXXXXXXXXXXXX'
```

We need to do this since the MTA won't search for 3rd party python library.

Any mail sent to `qualys@yourmailserver` will be send to the script.

## Requirements
- Python 3
- Slackweb library. `sudo pip install slackweb`

## Usage
```
usage: qualyswas-notification-to-slack.py [-h] -U SLACK_URL
                                          [-S {urgent,critical,serious,medium,minimal}]

Forwards Qualys WAS notification emails to Slack channel with severity control

optional arguments:
  -h, --help            show this help message and exit
  -U SLACK_URL, --slack-url SLACK_URL
                        URL of Slack Incoming Webhook
  -S {urgent,critical,serious,medium,minimal}, --severity {urgent,critical,serious,medium,minimal}
                        Severity on which the alert should be sent
```
