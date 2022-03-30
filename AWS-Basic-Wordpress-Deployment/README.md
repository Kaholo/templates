# AWS Basic Wordpress Deployment

![AWS-Basic-Wordpress-Deployment](/AWS-Basic-Wordpress-Deployment/aws-basic-wordpress-deployment.png)

This pipeline template enables you to deploy a standalone Wordpress environment within AWS by creating a new VPC and subnet within an existing AWS account, allocating an fixed (elastic) public IP address, creating an EC2 keypair and then deploying an AWS security group to control traffic in and out of the Wordpress instance. Finally, it can send a notification to a slack channel to indicate completion of the pipeline.

* **Create VPC**: The **Amazon EC2 Plugin** allows you to create a VPC by specifying the required parameters.
* **Create Subnet 1a**: This action creates a new subnet in the VPC.
* **Create default keypair**: This action creates the key pair required to connect to an Amazon EC2 instance.
* **Create security group**: Creates a security group to allow HTTP/HTTPS traffic.
    * **Add security group rules**: Creates an Ingress/Authorize security rule type for HTTP.
    * **Add security group rules**: Creates an Ingress/Authorize security rule type for HTTPS.
    * **Add security group rules**: Creates an Ingress/Authorize security rule type for SSH.
* **Create public ip**: Creates an AWS elastic IP to associate with the instance to ensure the public IP address of the Wordpress instances does not change.
* **Create Instance**: Creates a new Wordpress EC2 instance based on the AMI set in the Configuration.
* **Slack Send Message**: Sends a message to a Slack channel, notifying that the environment has been built.

A video on how to use the template can be found at https://youtu.be/E1dmn9xSk70

## Functions

If you need to process or transform your output from Actions, you can do it with functions defined in the Code tab. There, you can use JavaScript functions to call results from Actions and return the values. To get the result of a function, call it by turning on the Code toggle switch above a parameter field.

For example, the following function returns the VPC ID output from the Create VPC action:
```
function getVpcId(){
    return actions.createvpc.result.createVpc.Vpc.VpcId
}
```

To get the result of this function, use ```getVpcId()``` when configuring the action. 

**Functions used in this template:**

```
function getVpcId(){
    return actions.createvpc.result.createVpc.Vpc.VpcId
}

function getRouteTable(){
    return actions.createvpc.result.createRouteTable.RouteTable.RouteTableId
}

function getSubnet(){
    return actions.createsubnet1a.result.createSubnet.Subnet.SubnetId
}

function getKeyPair(){
    return actions.createdefaultkeypair.result.createKeyPair.KeyName
}

function getWordpressSecurityGroup(){
    return  `${actions.createwpsg.result.createSecurityGroup.GroupId}`
}

function getElasticIpId(){
    return actions.createPublicIp.result.allocateAddress.AllocationId
}

function getWordpressInstanceId(){
    return actions.createwordpressec2.result.runInstances.Instances[0].InstanceId
}
```

## Configuration variables

* **region**: The region to execute the request in e.g. _us-east-1_.
* **azA**: The availability zone to deploy the instance in e.g. _us-east-1a_.
* **genericTag**: The default tag to apply to resources being created e.g. _kaholo-basic-aws-wordpress_.
* **vpcTag**: The tag Name to apply to the VPC e.g _kaholo-wordpress-vpc_.
* **subnet1aTag**: The tag Name to apply to the subnet e.g. _kaholo-wordpress-vpc_.
* **vpcCidr**: The CIDR range to apply to the VPC e.g. _10.0.0.0/16_.
* **subnet1aCidr**: The CIDR range to apply to the Subnet e.g. _10.0.1.0/16_.
* **sshIngress**: The public IP address to allow inbound SSH connections from - recommended this is tied to your own public IP. Set to _0.0.0.0/0_ to allow inbound SSH from the entire internet (not recommended!).
* **keypairName**: The name to assign the Keypair to be created for SSH access to the Wordpress instance e.g. _kaholo-wordpress-keypair_.
* **wpAmi**: The image ID that provides the information required to launch a Wordpress instance. This can be found using the AWS Marketplace and the AMIs section in the AWS console _(NB -see the YouTube video link for instructions)_.
* **ec2Type**: The AWS machine type to assign the instance - this defines the amount of CPU / RAM assigned to the instance e.g. _t2.micro_.
* **slackChannel**: The name of the Slack channel to post notifications to. 

```
{
    "region": "",
    "azA": "",
    "genericTag": "Name=kaholo-basic-aws-wordpress",
    "vpcTag": "Name=kaholo-wordpress-vpc",
    "subnet1aTag": "Name=kaholo-wordpress-subnet-a",
    "vpcCidr": "10.0.0.0/16",
    "subnet1aCidr": "10.0.1.0/24",
    "sshIngress": "1.2.3.4/32",
    "keypairName": "kaholo-wordpress-keypair",
    "wpAmi": "",
    "ec2Type": "t2.micro",
    "slackChannel": "kaholo-notifications"
}
```
## Plugins used in this template

* [Amazon EC2](https://github.com/Kaholo/kaholo-plugin-amazon-ec2) Plugin for Kaholo
* [Slack](https://github.com/Kaholo/kaholo-plugin-slack) Plugin for Kaholo
* [Command Line](https://github.com/Kaholo/kaholo-plugin-cmd) Plugin for Kaholo
