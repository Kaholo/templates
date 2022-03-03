# Quarantine EC2 Instance by Security Trigger

Quarantine and snapshot EC2 instance template is designed to instantly respond to a security alert to your EC2 instance. Replace the "Trigger to get Instance" step with your monitoring plugin, which will return the instance ID.

### The description of the template works step by step (actions occur asynchronously):
* Creating and including the "Quarantine" group (restriction of all traffic). The purpose of this step is to prevent any information leaking from the instance during the snapshot and getting logs.
* Creating a snapshot
* Downloading the application/system logs from the instance, archiving and uploading them to the created S3 Bucket. You need to set up paths for your log files.
* Creating an incident in the PagerDuty notification system with a link to the log archive
* Stoping and terminating the instance after creating a snapshot and getting logs from it.

## Plugins used in the template
* [Amazon AWS EC2](https://github.com/Kaholo/kaholo-plugin-amazon-ec2) Plugin for Kaholo
* [Amazon AWS S3](https://github.com/Kaholo/kaholo-plugin-amazon-s3) Plugin for Kaholo
* [CommandLine](https://github.com/Kaholo/kaholo-plugin-cmd) Plugin for Kaholo
* [TextEditor](https://github.com/Kaholo/kaholo-plugin-textEditor) Plugin for Kaholo
* [PagerDuty](https://github.com/Kaholo/kaholo-plugin-pagerduty) Plugin for Kaholo
