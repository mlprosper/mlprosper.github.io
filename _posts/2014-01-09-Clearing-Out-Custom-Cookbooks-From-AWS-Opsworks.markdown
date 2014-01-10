---
layout: post
title: "Clearing Out Custom Cookbooks from AWS Opsworks"
---

Since starting to use AWS Opsworks for some of our deployments, we've run into some snags with the process. This particular one forced me to contact AWS support for tips.  I created a custom cookbook template as directed in the documention:

[http://docs.aws.amazon.com/opsworks/latest/userguide/workingcookbook-template-override.html](http://docs.aws.amazon.com/opsworks/latest/userguide/workingcookbook-template-override.html)

However, there was an error in my cookbook that was causing the deploys to fail, even after I tried reloading a previous successful deploy. Here's the error:

    [2014-01-09T23:14:50+00:00] DEBUG: Re-raising exception: Chef::Exceptions::ValidationFailed - Option type must be equal to one of: string, array, hash, symbol!  You passed "boolean".

Turns out, the cookbook is cached on the instance in a special location:

`/opt/aws/opsworks/current/site-cookbooks/`

Because my custom cookbook had a problem, Opsworks was not able to remove it properly and I had to clear it out yourself.  A simple `rm -r /opt/aws/opsworks/current/site-cookbooks` did the trick (I had to `sudo -i` first).

Fixing the custom cookbook template, updating the instances with the custom cookbook, then deploying again did the trick. 
