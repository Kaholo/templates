# AWS Basic Wordpress Deployment

This pipeline template enables you to deploy a Wordpress website to AWS EC2 by creating a new AWS EC2 instance and launching it into a VPC.

![aws-ec2-wordpress-deploy](https://i.imgur.com/90JAUcN.png)

* **Create VPC**: The **Amazon EC2 Plugin** allows you to create a VPC by specifying the required parameters.
* **Create default keypair**: This action creates the key pair required to connect to an Amazon EC2 instance.
* **Create Subnet**: This action creates a new subnet in the VPC. 
* **Create Instance**: Creates a new Wordpress EC2 instance.
* **Notify VPC**: Sends a message to a Slack channel, notifying that the VPC has been built.
* **Create security group**: Creates a security group to allow HTTP/HTTPS traffic.
* **Add security group rules**: Creates an Ingress/Authorize security rule type for HTTP.
* **Add security group rules**: Creates an Ingress/Authorize security rule type for HTTPS.


## Functions

If you need to process or transform your output from Actions, you can do it with functions defined in the Code tab. There, you can use JavaScript functions to call results from Actions and return the values. To get the result of a function, call it by turning on the Code toggle switch above a parameter field.

For example, the following function returns the output from the Create VPC action:
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
    return actions.createwpsg.result.createSecurityGroup[0]
}
```

## Configuration variables

* region: The region to execute the request in.
* ami: The image ID that provides the information required to launch an instance.

```
{
    "region": "eu-west-1",
    "ami": "ami-"

}
```
## Plugins used in this template

* [Amazon EC2](https://github.com/Kaholo/kaholo-plugin-amazon-ec2) Plugin for Kaholo
* [Slack](https://github.com/Kaholo/kaholo-plugin-slack) Plugin for Kaholo
