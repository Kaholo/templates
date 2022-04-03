# GCP Basic Wordpress Deployment

![GCP-Basic-Wordpress-Deployment](/gcp-basic-wordpress-deployment.png)

This pipeline template enables you to deploy a standalone Wordpress environment within GCP by creating a new VPC and subnet within an existing GCP project, allocating a fixed public IP address, and then deploying firewall rules to control traffic in and out of the Wordpress instance. Finally, it can send a notification to a slack channel to indicate completion of the pipeline.

* **Enable ... APIs**: The **_Google Cloud Services Plugin_** allows you to enable the GCP APIs that are required to deploy the resources programatically.
* **Create VPC**: The **_Google Cloud Compute Engine Plugin_** allows you to create a VPC by specifying the required parameters.
* **Create Subnet**: This action creates a new subnet in the VPC.
* **Create firewall rule**: Creates the firewall rules that allow inbound HTTP, HTTPS and SSH traffic.
* **Create Instance**: Creates a new Wordpress Compute Engine instance based on the _Image Name_ set in the Configuration.
* **Slack Send Message**: Sends a message to a Slack channel, notifying that the environment has been built.

A video on how to use the template can be found at https://youtu.be/xFwn7kNwsx8

## Configuration variables

* **gcpProject**: The GCP project name to deploy resources into _(can be found on the Home Dashboard within GCP)_.
* **gcpProjectNumber**: The GCP project number to deploy resources into _(can be found on the Home Dashboard within GCP)_.
* **region**: The region to execute the request in e.g. _europe-west2_.
* **zone**: The availability zone to deploy the instance in e.g. _europe-west2-a_.
* **genericTag**: The default tag to apply to resources being created e.g. _kaholo-basic-gcp-wordpress_.
* **vpcName**: The Name to apply to the VPC e.g _gcp-wordpress_.
* **subnetCidr**: The CIDR range to apply to the subnet  e.g. _10.0.1.0/24_.
* **wpImageProject**: The name of the project that hosts the Wordpress image e.g. _bitnami-launchpad_ for the Bitnami Wordpress images.
* **wpImageName**: The name of the image required to launch a Wordpress instance in GCP. This can be found using the _Google Cloud Marketplace_ and the _gcloud_ CLI _(NB - see the YouTube video link for instructions)_.
* **machineName**: The name to assign the Compute Engine instance that will be created e.g. _wordpress-01_.
* **machineType**: The GCP machine type to assign the instance - this defines the amount of CPU / RAM assigned to the instance e.g. _e2-small_.
* **slackChannel**: The name of the Slack channel to post notifications to.

```
{
    "gcpProject"        : "",
    "gcpProjectNumber"  : "",
    "region"            : "",
    "zone"              : "",
    "genericTag"        : "Name=kaholo-basic-gcp-wordpress",
    "vpcName"           : "gcp-wordpress",
    "subnetCidr"        : "10.0.1.0/24",
    "wpImageProject"    : "bitnami-launchpad",
    "wpImageName"       : "",
    "machineName"       : "wordpress-01",
    "machineType"       : "e2-small",
    "slackChannel"      : "kaholo-notifications"
}
```
## Plugins used in this template

* [Google Cloud Compute Engine](https://github.com/Kaholo/kaholo-plugin-GoogleCloudComputeEngine) Plugin for Kaholo
* [Google Cloud Services](https://github.com/Kaholo/kaholo-plugin-google-cloud-services) Plugin for Kaholo
* [Slack](https://github.com/Kaholo/kaholo-plugin-slack) Plugin for Kaholo
* [Command Line](https://github.com/Kaholo/kaholo-plugin-cmd) Plugin for Kaholo