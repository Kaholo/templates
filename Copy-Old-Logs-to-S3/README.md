# Copy Old Logs to S3

You can use this template to copy old logs to an S3 bucket based on the following logic:

![copy-logs-s3-bucket](https://i.imgur.com/s6Dhq7S.png)

1. Create a new directory using the **File System Plugin**. This is where your SSH key file will be stored. You can define a ```pathSsh``` configuration variable and call it from the **Path** field using ```kaholo.execution.configuration.pathSsh```.
2. Create a new file inside the directory using the **Text Editor Plugin**. The contents of the file is populated with the PEM key. You can use ```kaholo.execution.configuration.pem``` in the **File Content** field.
3. Using the **Command Line Plugin**, execute ```chmod 700 pem``` command to set read/write/execute permissions for the file containing the PEM key. 
4. Execute a script to move old logs to the S3 bucket. If you have more than 7 logs saved, then the script will start moving them to the S3 bucket from oldest to newest, leaving only 7 logs in the instance. To do this, create a new file using the **Text Editor Plugin** and add the script in the **File Content** field.

Script to move old logs to the S3 bucket:
```
    #!/bin/bash
    
    n=$(ls $path | wc -l)
    if [ $n -gt 7 ];
    then
            j=$(($n - 7))
            for ((i=1; i<=$j; i++))
            do
                old=$(ls -lt $path | tail -1 | awk '{ print $9 }')
                aws s3 mv $path/$old s3://$bucket/
            done
    fi
```
5. Save the script in a directory on the server using the **Command Line Plugin**. 
6. Run a **Remote Command Execution** to execute the script using the **Command Line Plugin**. The **Key Path**, **Remote User**, **Remote Address**, and **Command** fields can be filled in with the configuration variables.

## Plugins used in the template

* [File System Plugin](https://github.com/Kaholo/kaholo-plugin-fs) for Kaholo
* [Text Editor Plugin](https://github.com/Kaholo/kaholo-plugin-textEditor) for Kaholo
* [Command Line Plugin](https://github.com/Kaholo/kaholo-plugin-cmd) for Kaholo


## Configuration variables

* pathSsh - The path of the directory where the SSH file is created.
* pem - The path of the PEM key file.
* pathScript - The path of the script.
* pathLog - The path of the logs.
* bucket - The name of your S3 bucket.
* user - The SSH username used to authenticate to the remote host.
* host - The URL of the host to execute the command on.

Example:
```
{
    "pathSsh":"/home/user/sshkey",
    "pem": "/home/user/pem",
    "pathScript": "/home/user/script",
    "pathLog": "/home/user/logs",
    "bucket": "mybucket",
    "user": "myusername",
    "host": "remoteURL"
}
```
