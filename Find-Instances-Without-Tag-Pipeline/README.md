
#README

To make the script work please fill in values in the parameters Configurations:
- tagKey (Tag Key that should be present on instances. e.g. "kaholo" - the pipeline will check whether instances have {"kaholo": "any-value"} tag. Note: Pipeline does not check the value of the tag, only the key, but it can easily be added to the script - I left some comments in the Custom JS Code block)
- awsAccessKeyID (AWS Access Key for programmatic access to the AWS Account)
- awsSecretAccessKey (AWS Secret Access Key for programmatic access to the AWS Account)
- awsRegion (AWS Region where instances would be checked)

Pipeline uses 2 Command Line plugins because EC2 plugin currently does not support describe-instances AWS CLI call.
1st Command Line action calls describe-instances using AWS CLI.
2nd Command Line action calls the getInstanceIdsWithoutTag()function that parses the output of the first Action and returns the list of Instance IDs that do not have the specified tag key.
