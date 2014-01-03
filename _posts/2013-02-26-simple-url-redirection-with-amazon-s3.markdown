---
layout: post
title: "Simple URL Redirection with Amazon S3"
date: 2013-02-26 09:38
comments: true
categories: 
---

We use Google Apps for Business at our company for email.  For the most part we are happy, but some things are just plain annoying. Take for instance, the use of multiple domain names with a single Google Apps account. If you set up the account with a domain name and want to change it later, the process is excruciating. In our instance we have `mail.domain1.com` set up on our account as a primary that we want to phase out. We want all our users to go to `mail.domain2.com` on our secondary domain. However, Google Apps only allows us to a use a CNAME on a subdomain of the **primary.

To work around this limitation, I had to set up a redirect so that when users go to `mail.domain2.com`, they get redirected to the Google Apps sign in URL: https://www.google.com/a/domain2.com/. Luckily Amazon Web Services introduced a feature that allows us to set up simple static websites on their Simple Storage Service (S3).

The first step is to create a bucket in the Management Console and enable Static Website Hosting. You'll have to set the bucket to redirect all requests to another host name:

<div style="margin-left:10px;"><img alt="reloaded" src="/images/s3_redirection.png"/></div>

Step 2 is to set up a CNAME in either Route53 (if your Nameservers are on AWS) or on your Domain Registrar that points to the bucket endpoint:

<div style="margin-left:10px;"><img alt="reloaded" src="/images/route53_domain_settings.png"/></div>


Once you do that, you have a very simple, easy to configure redirect from one url to another.

For more information about hosting static websites on Amazon S3, click here: 

<a href="http://aws.typepad.com/aws/2011/02/host-your-static-website-on-amazon-s3.html">http://aws.typepad.com/aws/2011/02/host-your-static-website-on-amazon-s3.html</a>