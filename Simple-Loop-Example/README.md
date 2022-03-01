# Simple-Loop-Example

This pipeline template includes a simple example on how to create a loop on an Action using the Conditional Code and Post Function features to control the flow.

![loop-example](https://i.imgur.com/ASVGFcE.png)

* **Loop**: This Action executes the ```echo "Hello ${i}"``` command using the CMD Plugin. 

## Variables and Conditions

* ```let i =0;``` - defined in the Code tab
* ```i<6``` - added in the Conditional Code field
* ```i++``` - added in the Post-Execution Function field 

## Plugins used in this template

* [Command Line](https://github.com/Kaholo/kaholo-plugin-cmd) Plugin for Kaholo
