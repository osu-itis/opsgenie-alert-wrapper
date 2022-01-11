# opsgenie-alert-wrapper

A python wrapper that creates an OpsGenie alert when a script fails. Wrapped scripts must return [proper bash exit codes](http://tldp.org/LDP/abs/html/exit-status.html) (0 on success, non-zero for failure). Output from `stderr` will be captured and sent in the alert description.

## Requirements

* Python 3.8+
* `shellpy`
* `opsgenie-sdk`
* `pyyaml`

## Configuration

Configuration is performed via yaml config file:

```
alert_message - string to send as alert message
alert_source - string to send as alert source
alert_description - string to send as alert description
alert_teams - yaml formatted array of team objects to assign to alert (req. name, type)
target_script_home - path to target script's dir
target_run_command - command to run
alertfile_dir - path to dir for storing .alert files
```

The Opsgenie API key must be provided as environment variable: ```OPSGENIE_API_KEY```

## Usage

```
$ shellpy opsgenie-alert-wrapper.spy
```

## Example

Say we have a ruby script called `checklogin.rb` that attempts to login to a system and returns 0 on success and 1 on failure. The script is located in `/opt/scripts/monitor/`.

Sample yaml config:
```
---
alertfile_dir: "/tmp"
target_run_command: 'ruby checklogin.rb'
alert_source: checklogin.rb
target_script_home: "/opt/scripts/monitor"
alert_description: checklogin script failed
alert_message: Login to somehost.edu has failed
alert_teams:
  - name: someteam
    type: team
  - name: foobar
    type: user
```

## Jenkins

`opsgenie-alert-wrapper` works well when executed via Jenkins jobs. Use the [Config File Provider Plugin](https://plugins.jenkins.io/config-file-provider/) to provide script config vars on a per-job basis. Secure credentials such as the API key are provided by the [Credentials Binding Plugin](https://plugins.jenkins.io/credentials-binding).
