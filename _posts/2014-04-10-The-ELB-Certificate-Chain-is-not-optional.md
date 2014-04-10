---
layout: post
category: devops
title: "The ELB Certificate Chain is not Optional"
---

At CaseNEX, we make extensive use of Amazon Web Services and Elastic Load Balancers to handle SSL. Since the SSL certificate is installed directly on ELB, none of our servers need to be individually configured. This is a great boon to DevOps productivity, but there are a couple caveats when installing certs...

### 1. The Certificate Chain is not always optional

When installing an SSL cert to your ELB, the screen to enter the cert info looks like this:

![cert information](https://dl.dropboxusercontent.com/u/11024433/Screenshots/2014-04-10%20at%2010.35.55%20AM.png)

 The first and second boxes are required, but the third is not. However, if you are installing a cert from a CA, it would be wise of you to place in the box the certificate chain sent to you along with your SSL cert. For example, from Comodo, the email could look like this when you receive the cert via email:

  ![email](https://dl.dropboxusercontent.com/u/11024433/Screenshots/2014-04-10_12-58-33.png)

Your certificate chain is the combination of the root and intermediate certificates. Literally copy and paste the contents of both files into the Certificate Chain box. The order *does* matter. You must paste the intermediate certificate first or you will get this message:
![error](https://dl.dropboxusercontent.com/u/11024433/Screenshots/2014-04-10%20at%2010.42.53%20AM.png)

Failing to install the certificate chain can lead to random client browsers getting an "Untrusted Connection". Also, any OAuth2 clients that use the Faraday gem might run into an obscure SSL error:

```
SSL_connect returned=1 errno=0 state=SSLv3 read server certificate B: certificate verify failed
```

### 2. The SSL Terminates at the ELB

This was a hard concept for some of our devs that were always used to installing the SSL certificate on the web server itself (IIS users know this very well.) Since the SSL certificate is installed on the load balancer, SSL is terminated at the ELB and the application *has no knowledge that the traffic is using SSL*. This means that any code detecting and handling SSL traffic in the application is now obsolete.

Overall we've been really satisfied with the performance and reliability of ELB and will continue using it for forseeable future.