# Build Docker and Deploy

You can use this template to build a Docker image and deploy it based on the following logic:

![build-docker-and-deploy](https://i.imgur.com/EZLMWvj.png)

1. Clean up your working directory using the **File System Plugin**. The **Delete File/Directory** method allows you to remove any old build directory.
2. Clone the repository using the **git Plugin**.
3. With the **Command Line Plugin**, execute command ```ls -lh /home/node``` to list the files or directories of a specified path. In this case, ```/home/node``` is the local path where the git repository was previously cloned. 
4. Ensure the **dockerfile** exists in the specified working directory. With the **File System Plugin** you can use the **Path Exists** method to check whether a file exists or not in a given path.
5. Login to Docker. The **Docker Plugin** has a **Docker CMD** method that provides a general command line to execute any Docker command. 

  Here's an example:
```
    kaholo.vault.getValueByKey('registry_password')
     .then(function(value){
      return `login -u nowak.kaholo.net -p ${value} testreg.vodolaz095.life`;
    });
```
6. Build the Docker image. Specify the path of the dockerfile to build the image from. You can also add a tag to the image, such as ```latest```.
7. Push the container to the registry. Specify the image name, image tag, and registry. If the registry is not specified, the image is pushed to Docker Hub registry by default.
8. Call a webhook using the **HTTP Requests Plugin** to force cluster update. 

## Plugins used in the template

* [File System Plugin](https://github.com/Kaholo/kaholo-plugin-fs) for Kaholo
* [git Plugin](https://github.com/Kaholo/kaholo-plugin-git) for Kaholo
* [Command Line Plugin](https://github.com/Kaholo/kaholo-plugin-cmd) for Kaholo
* [Docker Plugin](https://github.com/Kaholo/kaholo-plugin-docker) for Kaholo
* [HTTP Requests Plugin](https://github.com/Kaholo/kaholo-plugin-http-requests) for Kaholo
