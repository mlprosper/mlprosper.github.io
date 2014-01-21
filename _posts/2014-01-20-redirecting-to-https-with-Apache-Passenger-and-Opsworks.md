---
layout: post
title: "Redirecting to HTTPS with Apache, Passenger and Opsworks"
---

Our AWS Opsworks stack uses the common Apache and Passenger combination to serve one of our Ruby on Rails apps. This app sits behind an Elastic Load Balancer (ELB) that distributes traffic between our web instances as well as handles SSL termination.

One of the tricky aspects was making sure that all traffic to the app came over HTTPS. Any traffic over HTTP needed to be redirected.  To do this, we added a couple of lines to the stock `web_app.conf` in the AWS `apache_passenger` recipe:

     # if using Amazon ELB with SSL Termination
     RewriteCond %{HTTP:X-Forwarded-Proto} !https
     RewriteRule !/status https://%{SERVER_NAME}%{REQUEST_URI} [L,R]

This small snippet checks for the existence of the X-FORWARDED-PROTO header that the ELB attaches to every request. That header contains either "http" or "https" depending on the protocol of the original request to the ELB. [Click here](http://docs.aws.amazon.com/ElasticLoadBalancing/latest/DeveloperGuide/TerminologyandKeyConcepts.html#x-forwarded-headers) for more info about the special headers.

The second line rewrites the request to https as long as it doesn't match the ping target of the ELB. It's important to exclude that target so that the health check for the ELB doesn't fail. The health check for the ELB is set in the properties screen:

![](https://dl.dropboxusercontent.com/u/11024433/Screenshots/2014-01-20_21-54-32.png)

After we those lines to the custom recipe, we added the recipe to the list of custom recipes to run in Opsworks:

![](https://dl.dropboxusercontent.com/u/11024433/Screenshots/2014-01-20_15-54-57.png)

To roll out the changes, we had to replace the current instances with new ones so the Setup recipes would be run. [Click here](https://github.com/CaseNEX/opsworks-cookbooks/blob/master-chef-11.4/passenger_apache2/templates/default/web_app.conf.erb#L58) to see the full template in our custom recipe.