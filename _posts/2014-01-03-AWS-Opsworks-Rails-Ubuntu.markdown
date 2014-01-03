---
layout: post
title: "AWS OpsWorks with Rails, Ubuntu, Apache and Passenger"
---
We've been trying to simplify the deployment processes of our Rails apps at work and decided to try Amazon OpsWorks. So far, we've not been all that successful.

###Ubuntu 12.04
First, we tried creating a layer with these options:

{<1>}![a](https://dl.dropboxusercontent.com/u/11024433/Screenshots/Screenshot%202014-01-03%2010.51.37.png)

We booted a 64-bit Ubuntu 12.04 LTS instance successfully.  The Rails app was running, everyone was happy.

Then we tried to stop and start the instance.

On restarting, the instance failed with the following error:


    [2014-01-03T18:26:32+00:00] DEBUG: template[/etc/logrotate.d/apache2] content has not changed.
    [2014-01-03T18:26:32+00:00] INFO: Processing execute[logdir_existence_and_restart_apache2] action run (    apache2::default line 191)
    [2014-01-03T18:26:33+00:00] INFO: Running queued delayed notifications before re-raising exception
    [2014-01-03T18:26:33+00:00] DEBUG: Re-raising exception: Mixlib::ShellOut::ShellCommandFailed - execute[    logdir_existence_and_restart_apache2] (apache2::default line 191) had an error:     Mixlib::ShellOut::ShellCommandFailed: Expected process to exit with [0], but received '2'
    ---- Begin output of ls -la /var/log/apache2 ----
    STDOUT: 
    STDERR: ls: cannot open directory /var/log/apache2: No such file or directory
    ---- End output of ls -la /var/log/apache2 ----
    Ran ls -la /var/log/apache2 returned 2
  
Looks like the log directory for Apache is not being found, causing the `ls` command to dump the whole setup process.


###Amazon Linux
We also tried the Amazon Linux distribution, but ran into a different error with the postgres gem:

    Installing pg (0.17.1) 
    Gem::Installer::ExtensionBuildError: ERROR: Failed to build gem native extension.
     
    /usr/local/bin/ruby extconf.rb 
    checking for pg_config... no
    No pg_config... trying anyway. If building fails, please try again with
    --with-pg-config=/path/to/pg_config
    checking for libpq-fe.h... no
    Can't find the 'libpq-fe.h header
    *** extconf.rb failed ***
    Could not create Makefile due to some reason, probably lack of
    necessary libraries and/or headers.  Check the mkmf.log file for more
    details.  You may need configuration options.
   
This can be fixed by adding `libpq-dev` to the packages installed during setup:
{<2>}![](https://dl.dropboxusercontent.com/u/11024433/Screenshots/2014-01-03_11-02-39.png)

But this causes yet another problem with that library. We gave up at this point and just went back to Ubuntu.


###What Now?

We're waiting on AWS support to give us a resolution to the Chef setup error on Ubuntu.  I'll update here when we know more.