# Passing-data-example

This pipeline template includes a basic example demonstrating how to pass data between Actions in a pipeline.

![passing-data-between-actions](https://i.imgur.com/MhmrRJJ.png)

**Steps:**
* **Action 1**: This action executes the ```getCmd1()``` command using the CMD Plugin. 
* **Action 2**: This action executes the ```getCmd2()``` command using the CMD Plugin.

## Functions 

The following functions have been defined in the Code tab and demonstrate how to pass data between Actions. In this example, the first function returns the output of an Action, which is then being passed to the second Action.

```
function getCmd1(){
    return `echo "cmd1 Output"`
}

function getCmd2(){
    return `echo "${actions.action1.result} from Cmd2"`
}
```

## Plugins used in this template

* [Command Line](https://github.com/Kaholo/kaholo-plugin-cmd) Plugin for Kaholo
