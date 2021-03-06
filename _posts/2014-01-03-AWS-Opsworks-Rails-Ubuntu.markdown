---
layout: post
title: "AWS OpsWorks with Rails, Ubuntu, Apache and Passenger"
---
We've been trying to simplify the deployment processes of our Rails apps at work and decided to try Amazon OpsWorks. So far, we've not been all that successful.

###Ubuntu 12.04
First, we tried creating a layer with these options:

![a](https://dl.dropboxusercontent.com/u/11024433/Screenshots/Screenshot%202014-01-03%2010.51.37.png)

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
![](https://dl.dropboxusercontent.com/u/11024433/Screenshots/2014-01-03_11-02-39.png)

But this causes yet another problem with that library. We gave up at this point and just went back to Ubuntu.


### What Now?

We're waiting on AWS support to give us a resolution to the Chef setup error on Ubuntu.  I'll update here when we know more.

#### Update: AWS has found a problem with their Chef recipe located here: (https://github.com/aws/opsworks-cookbooks/blob/master-chef-11.4/apache2/recipes/default.rb)

Their solution is to remove the last line of the `/etc/auto.master` file. Apparently this forces autofs to recheck mounted volumes and removed the ability for the error to appear. To remove the line from that file, run this simple command on the instance:

    sudo sed -i /opsworks/d /etc/auto.master

Then restart the instance.    

#### Update 2: After an extensive chat with AWS support, they've escalated the issue in the cookbook with the Opsworks team, and I should get an update when they fix the recipe. In the meantime, the easier workaround is to use Instance-store when creating a new instance:
![](https://dl.dropboxusercontent.com/u/11024433/Screenshots/2014-01-15_16-42-24.png)

#### Update 3: They've updated the cookbook and now you can restart an EBS instance without a problem.  Here's the full text of their email:

        The Opsworks team have released a fix for this bug in Opsworks agent version 219

        Your current instances should automatically update to the latest Opsworks agent , and any new instances should receive the new agent version 219.

        The process to check the Opsworks agent version is quite simple, and can be done with a simple command once ssh'd into the instance:

        "sudo opsworks-agent-cli agent_report"

        If your instances are reporting a different value than 219 for the Opsworks agent version, please let us know and we can have the Opsworks team apply that version to your stacks.

        There is a manual way to update existing instances to the new version. The proccess is as follows:

        - Change the target version in the instance:

        sudo echo '219' > /var/lib/aws/opsworks/TARGET_VERSION

        - Then run this command to update the agent:

        sudo sh -c 'cd /opt/aws/opsworks/current && bin/lockrun --wait --verbose --lockfile=/var/lib/aws/opsworks/lockrun.lock -- /opt/aws/opsworks/current/bin/opsworks-agent-updater'

