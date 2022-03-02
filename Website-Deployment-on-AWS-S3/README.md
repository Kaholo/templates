# Website-Deployment-on-AWS-S3

This pipeline template can be used to deploy a static website to an Amazon S3 bucket. It clones a specified git repository and then creates a new AWS S3 bucket. The bucket policy in this pipeline ensures that the bucket is accessible to the public. Once the static website hosting is enabled and the source files are uploaded to the bucket, you can send a Slack message to notify a channel or a user that the operation has been successful.  

![website-deployment-aws-s3](https://i.imgur.com/tHwjru1.png)

* **Git clone**: Clones a specified repository using the CMD Plugin. In this example, the command to execute is ```git clone git clone https://github.com/mahendra1904/static-webpage-example-master.git```. Make sure to provide the correct URL address for cloning the repository. Alternatively, you can use the [git Plugin](https://github.com/Kaholo/kaholo-plugin-git) to clone public/private repositories. 
* **Create Bucket**: Creates a new S3 bucket on AWS. 
* **Public Access**: This Action uses the **Manage Public Access Block** method and enables the block public access settings for the specified bucket.
* **Bucket Policy**: Applies the following policy to the specified bucket.
```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "PublicReadGetObject",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::static-bucket-host/*"
    }
  ]
}
```
* **Enable Static Website Hosting**: This Action enables static website hosting for the specified bucket. 
* **Upload File**: Uploads files to the bucket using the CMD Plugin. In this example, the command to execute is ```aws s3 sync . s3://static-bucket-host```. Make sure to fill in the correct URL. The path in which the command will be executed from is also required, e.g. /aws/static-webpage-example-master/src. 
* **Slack Notification**: Sends a Slack message to a channel or a user, e.g. "Successful". 

## Plugins used in this template

* [AWS S3](https://github.com/Kaholo/kaholo-plugin-amazon-s3) Plugin for Kaholo
* [Command Line](https://github.com/Kaholo/kaholo-plugin-cmd) Plugin for Kaholo
* [Slack](https://github.com/Kaholo/kaholo-plugin-slack) Plugin for Kaholo
