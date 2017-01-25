# opsgenie-alert-wrapper

A python wrapper that creates an OpsGenie alert when a script fails. Wrapped scripts must return [proper bash exit codes](http://tldp.org/LDP/abs/html/exit-status.html) (0 on success, non-zero for failure). Output from `stderr` will be captured and sent in the alert description.

## Requirements

* `shellpy >= 0.5.0`
* `opsgenie-sdk >= 0.2.1`

## Configuration

Configuration is performed via environment variables:

```
OPSGENIE_API_KEY - (required) OpsGenie API key
ALERT_MESSAGE - string to send as alert message
ALERT_SOURCE - string to send as alert source
ALERT_DESCRIPTION - string to send as alert description
ALERT_TEAMS - comma-separated list of teams to assign to alert
TARGET_SCRIPT_HOME - path to target script's dir
TARGET_RUN_COMMAND - command to run
ALERTFILE_DIR - path to dir for storing .alert files
```

## Usage

```
$ shellpy opsgenie-alert-wrapper.spy
```

## Example

Say we have a ruby script called `checklogin.rb` that attempts to login to a system and returns 0 on success and 1 on failure. The script is located in `/opt/scripts/monitor/`.

Sample env config:
```
OPSGENIE_API_KEY=xxxxxxxxxxx
ALERT_MESSAGE=checklogin failed
ALERT_SOURCE=checklogin.rb
ALERT_DESCRIPTION=login to somehost.edu has failed
ALERT_TEAMS=someteam
TARGET_SCRIPT_HOME=/opt/scripts/monitor
TARGET_RUN_COMMAND=ruby checklogin.rb
ALERTFILE_DIR=/tmp
```

## Jenkins

`opsgenie-alert-wrapper` works well when executed via Jenkins jobs. Use the [EnvInject Plugin](https://wiki.jenkins-ci.org/display/JENKINS/EnvInject+Plugin) to define environment vars on a per-job basis.
