# AWS-VPC-with-DMZ

This pipeline template uses the AWS EC2 service and creates a VPC and the corresponding subnets, and then creates the route table - one that connects DMZ to the Internet gateway and another through the NAT instance. 

![aws-vpc-dmz](https://i.imgur.com/fLqHLHA.png)

* **Create VPC**: Creates a VPC with the specified IPv4 CIDR block.
* **Create Subnets**: Creates the subnets within the VPC.
* **Create Route Table**: Creates one route that connects DMZ to the Internet gateway and another one through the NAT instance.
* **Associate Route**: Sets up a route for the DMZ subnet.

## Functions and variables

This pipeline template includes the following functions and variables that are defined in the Code tab.

```
var conf = kaholo.execution.configuration
var codeRegion = kaholo.execution.configuration.Region
var codeAccessKey = kaholo.vault.getValueByKey(kaholo.execution.configuration.VaultedAWSKeyName)
var codeSecretKey = kaholo.vault.getValueByKey(kaholo.execution.configuration.VaultedAWSSecretName)
var codeVpcTags = "Name=" + conf.VPC.Name
var codeVpcCidr = kaholo.execution.configuration.VPC.CIDR
var conf = kaholo.execution.configuration
var codeVpcId = kaholo.execution.configuration.VPC.VpcId
var codeVpcExists = !(codeVpcId === "")
```

```
var stashedVpcId = ""
var stashedSubnetIds = ["","","","",""]
var stashedDmzRouteTableId = ""
var stashedNatRouteTableId = ""
```

```// Subnet loop
var subnetLoopVar = 0;
var subnetLength = kaholo.execution.configuration.Subnets.length
var keepGoingCondition = subnetLoopVar < subnetLength
```

```// Subnet PostExecution
function subnetPostExe() {
    stashedSubnetIds[subnetLoopVar] = kaholo.actions.awsCreateSubnet.result.createSubnet.Subnet.SubnetId;
    subnetLoopVar++;
}
```

## Configuration variables
```
{
    "VaultedAWSKeyName": "terrakey5S",
    "VaultedAWSSecretName": "terrasecret5S",
    "Vaulted_comment": "The above two items are Names (items) from the vault. Use Settings | Vault.",
    "Region": "ap-southeast-1",
    "VPC": {
        "Name": "Alpha Net",
        "CIDR": "10.52.88.0/24",
        "VpcId": "",
        "VpcId_comment": "if VpcId is empty string, VPC will get created."
    },
    "Subnets": [
        {
            "Name": "DMZ",
            "CIDR": "10.52.88.0/26",
            "AZ": "ap-southeast-1a"
        },
        {
            "Name": "NetA",
            "CIDR": "10.52.88.64/26",
            "AZ": "ap-southeast-1a"
        },
        {
            "Name": "NetB",
            "CIDR": "10.52.88.128/26",
            "AZ": "ap-southeast-1b"
        },
        {
            "Name": "NetC",
            "CIDR": "10.52.88.192/26",
            "AZ": "ap-southeast-1c"
        }
    ]
}
```
# Plugins used in this template

* [AWS EC2](https://github.com/Kaholo/kaholo-plugin-amazon-ec2) Plugin for Kaholo
