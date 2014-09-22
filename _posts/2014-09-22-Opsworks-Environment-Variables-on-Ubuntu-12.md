---
layout: post
category: devops
title: "Opsworks Environment Variables on Ubuntu 12.04"
---
Since switching a number of applications at CaseNEX to Opworks we've run into a few quirks. The latest one involved not being able to access environment variables from a ruby app when set in the Opsworks console.



Amazon [announced](http://blogs.aws.amazon.com/application-management/post/Tx2G95J53SKI2E0/AWS-OpsWorks-supports-application-environment-variables) the ability to set environment variables in Opsworks a few months ago. Normally, we can set environment variables in the App settings:
![s](https://dl.dropboxusercontent.com/u/11024433/Screenshots/Screenshot_2014-09-22_13.22.41.png)

However, one of our applications refused to recognize the variables we set in the console. Every time we accessed the variable in Rails it was blank. We tried rebooting, changing the bundler and passenger version, and deploying different types of instances to no avail. 

We noticed that our Opsworks stack had a setting for using Custom Cookbooks. We used this feature extensively in other Opsworks stacks. Here we had custom cookbooks turned on but weren't actually using any recipes. So we turned it off:

![s](https://dl.dropboxusercontent.com/u/11024433/Screenshots/Screenshot_2014-09-22_13.13.22.png)

That did it. Turning off the custom cookbooks made the environment variables magically appear. I have no idea why. Hope that saves someone else a few minutes.