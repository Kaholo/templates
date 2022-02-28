# Angular-Deployment-to-S3-bucket

This pipeline template builds an Angular App from source code using AWS CodeBuild, creates a pipeline using AWS Codepipeline, and deploys it in an AWS S3 bucket. The S3 bucket acts as a static web hoster using an AWS S3 Bucket policy that will make the bucket accessible by the public. The AWS Codebuild Plugin for Kaholo will pull the code from the Github repository and do the necessary build process and Codebuild will then push the resulting artifacts to the S3 bucket.

![angular-deployment-s3](https://i.imgur.com/3VfzsJL.png)

* **Create S3 Bucket**: Creates a new S3 bucket on AWS that will store all the files of the application with the **Create Bucket** method of the AWS S3 Plugin.
* **S3 Enable Versioning**: Enables the versioning of the bucket that can be used to preserve, retrieve, and restore every version of every object stored in your Amazon S3 bucket. Uses the **Manage Bucket Versioning** method of the AWS S3 Plugin.
* **S3 Policy**: Applies a specified policy to the S3 bucket. A bucket policy can be used to grant access permissions to the bucket and the objects in it. This policy will grant public read access for the website.
* **Enable Static Website Hosting**: Enables the bucket to host a website and configures a bucket as a static website. Uses the **Enable Bucket Website Hosting** method of the AWS S3 Plugin.
* **Create Github Repo Webhook**: The webhook will trigger as soon as there will be any change in the source code in the specified repository. Uses the **Create Repository Webhook** method of the Github Plugin. 
* **Create CodeBuild Project**: The AWS CodeBuild Plugin will build the project as a fully managed continuous integration service that compiles source code, runs tests, and produces software packages that are ready to deploy. Uses the **Start Build** method.
* **Store S3 Bucket**: Creates a new S3 bucket that will store all the source files of the application and the artifacts of the AWS CodeBuild.
* **AWS CodePipeline JSON**: Creates the AWS CodePipeline from JSON file. Uses the **Create Pipeline from JSON** method of the AWS CodePipeline Plugin. 
* **Slack Notification**: Sends a Slack message using the **Send incoming webhook** method of the Slack plugin. 

## Plugins used in the template

* [Amazon AWS S3](https://github.com/Kaholo/kaholo-plugin-amazon-s3) Plugin for Kaholo
* [Github](https://github.com/Kaholo/Kaholo-plugin-github) Plugin for Kaholo
* [AWS CodeBuild](https://github.com/Kaholo/kaholo-plugin-aws-code-build) Plugin for Kaholo
* [AWS CodePipeline](https://github.com/Kaholo/kaholo-plugin-aws-codepipeline) Plugin for Kaholo
* [Slack](https://github.com/Kaholo/kaholo-plugin-slack) Plugin for Kaholo
