---
layout: post
title: "Passing the real client IP to your Apache logs in Opsworks"
---

An annoying default when using the apache2 recipe with an Elastic Load Balancer (ELB) in Opsworks is that the logs will record the source IP as the ELB instead of the actual client ip. The ELB passes the originating IP as a custom header in the request called "X-Forwarded-For". By default the logs end up looking like this:

![s](https://dl.dropboxusercontent.com/u/11024433/Screenshots/2014-03-20_17-14-25.png)

Not very useful if you need to use the actual client ip for anything. To change this, we need to write a custom template for the apache2 recipe in the aws opsworks-cookbooks repo.

Take the apache2/templates/default/apache2.conf.erb file and copy it to your custom cookbook repo. Then change the topmost LogFormat lines like this (I've included all the lines for reference):
![](https://dl.dropboxusercontent.com/u/11024433/Screenshots/2014-03-20_17-23-44.png)

Commit the change, then update the cookbooks on AWS Opsworks by going to Run Command in the Stack Settings:
![](https://dl.dropboxusercontent.com/u/11024433/Screenshots/2014-03-20_16-55-17.png)

After that, you'll have to deploy your code one more time. If you tail the logs now, you should see the client IP show up:
![](https://dl.dropboxusercontent.com/u/11024433/Screenshots/2014-03-20_17-14-45.png)

