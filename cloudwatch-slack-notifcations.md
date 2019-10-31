# AWS CloudWatch Alarms to Slack
This is a step-by-step guideline to setup an Alarm monitoring service on AWS using CloudWatch, which will forward its notifications to Slack.
## Table of Contents
1. [Dependencies](#dependencies)
2. [Wallet Setup](#wallet-setup)
3. [Node Initiation](#node-initiation)
4. [Basic Usage](#basic-usage)

## Dependencies
Install [Go](https://github.com/EOSIO/eos) programming language.

Install [AWS CLI](https://github.com/EOSIO/eosio.cdt) (for testing purposes).

## Deploy Lambda Function
```sh
go get "github.com/aws/aws-lambda-go/lambda"
cd $LAMBDA_FUNCTION_DIRECTORY
chmod 755 build.sh
./build.sh
```

```sh
aws cloudwatch set-alarm-state --alarm-name "[ICON] - mainnet - PRep - backup - CPU Alert" --state-reason "test" --state-value OK
aws cloudwatch set-alarm-state --alarm-name "[ICON] - mainnet - PRep - backup - CPU Alert" --state-reason "test" --state-value ALARM
```

https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-configure.html

https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/mon-scripts.html

## Configure Memory and Disk Monitoring Metrics
Install required packages:
```sh
sudo apt-get update
sudo apt-get install unzip
sudo apt-get install libwww-perl libdatetime-perl
```
Download monitoring scripts:
```sh
curl https://aws-cloudwatch.s3.amazonaws.com/downloads/CloudWatchMonitoringScripts-1.2.2.zip -O
```
Install the monitoring scripts:
```sh
unzip CloudWatchMonitoringScripts-1.2.2.zip && \
rm CloudWatchMonitoringScripts-1.2.2.zip && \
cd aws-scripts-mon
```
Configure AWS profile:
```sh
nano awscreds.conf
```
Below is an example AWS Profile:
```
AWSAccessKeyId=AKIAIOSFODNN7EXAMPLE
AWSSecretKey=wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY
```
```sh
## Available Disk Space
./mon-put-instance-data.pl --disk-space-util --disk-path=/
```
## Setup Cron Schedule
```sh
crontab -e
```
Below is an example of cron jobs:
```sh
## Report memory utilization to CloudWatch every 1 minutes:
*/1 * * * * ~/aws-scripts-mon/mon-put-instance-data.pl --mem-used-incl-cache-buff --mem-util --mem-used --mem-avail --memory-units=gigabytes --disk-space-util --disk-space-used --disk-space-avail --disk-path=/ --from-cron
```