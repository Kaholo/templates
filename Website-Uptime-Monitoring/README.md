# Website-uptime-monitoring

This pipeline template monitors website health.

![website-uptime-monitoring](https://i.imgur.com/lvxRAAY.png)

* **GET**: Sends an HTTP request to the specified URL. In this example, the URL to send the request to is https://geofeed.vpnsvc.com/. 
* **Save current state**: Saves the current check result of the website to a new file. You can get the file content by accessing the result of the previous Action: ```""+actions.httprequests1.result.statusCode```.
* **Touch file**: Creates a new file with the previous status of the website. Executes the ```touch ./website_status_previous.txt``` command using the CMD Plugin.
* **Compare states**: Runs a comparison between current website check result with previous result. Executes the ```diff ./website_status.txt ./website_status_previous.txt | wc -l``` command using the CMD Plugin.
* **Send alert**: Send a Slack message containing the status result to a specified user.

  This Action is executed based on the following condition: ```actions.commandline1.result !='0'```. 

  You can then get the message content by accessing the result of a previous Action:
    ```"Site responds "+actions.httprequests1.result.statusCode```
* **Save status**: Copies the website status into a specified destination path. 

## Plugins used in this template
 
* [HTTP Requests](https://github.com/Kaholo/kaholo-plugin-http-requests) Plugin for Kaholo
* [Text Editor](https://github.com/Kaholo/kaholo-plugin-textEditor) Plugin for Kaholo
* [Command Line](https://github.com/Kaholo/kaholo-plugin-cmd) Plugin for Kaholo
* [Slack](https://github.com/Kaholo/kaholo-plugin-slack) Plugin for Kaholo
* [File System](https://github.com/Kaholo/kaholo-plugin-fs) Plugin for Kaholo
