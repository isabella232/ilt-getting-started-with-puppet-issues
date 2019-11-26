# Lab 2.2: Running commands

In this lab you will learn how to:

* Use Bolt to run simple commands on a target machine.
* Use Bolt to run existing scripts, written in any language, on a target machine.

# Setup

You will need two machines to run Bolt on. These should be the two machines you installed Bolt on in the previous lab.

**_For the remainder of this lab, you will use instructions for both [Windows](../../Windows/lab-2.2-Running-Bolt-Commands) and Linux. Follow both sets of instructions, so you get experience running Bolt on both platforms._**

# Steps

### Use Bolt to run a command on the local machine

You are going to stop the time service on the Windows and Linux machines machine you are connected to, by running a system command on those machines.

#### Use Bolt on a Linux machine to run a command on that machine

Use SSH to connect to the Linux machine you will run Bolt on. Once you are connected, open a shell terminal on that machine, and then run this command within the terminal:

```$ bolt command run 'sudo systemctl stop ntpd' --targets ssh://localhost --user centos --no-host-key-check```

**_The required SSH key setup has been built into the student Linux machine, so you do not need to supply a password._**

**_The `--no-host-key-check` option lets the command run even though without an entry in the SSH `known_hosts` file. However, the default behavior of Bolt is to validate SSH key signatures for security purposes._**

The output will be similar to this:

```
Started on localhost...
Finished on localhost:
Successful on 1 node: ssh://localhost
Ran on 1 node in 0.45 seconds
```

# Pause here and run the [Windows](../../Windows/lab-2.2-Running-Bolt-Commands) Exercise before continuing.

### Use Bolt to run remote system commands

Now you will start the time service on a remote machine, which is a bit more interesting.

**_For this exercise you will use your *other* student machine. For example, if you are logged into your Windows machine, you will start the service on your Linux machine._**

#### Start the time service

##### Use Bolt on a Linux machine to run a command on a Windows machine

On your Linux machine, run the following in a terminal window:

```$ bolt command run 'net start w32time' --targets winrm://<your-windows-machine>.classroom.puppet.com --user Administrator --no-ssl --password```

**_You will be prompted for a password._**

The output will look similar to the following:

```
STDOUT:
   The Windows Time service is starting.
   The Windows Time service was started successfully.
```

#### Validate that the time service is running

##### Use Bolt on a Linux machine to run a command on a Windows machine

On your Linux machine, run the following in a terminal window:

```$ bolt command run 'Get-Service w32time' --targets winrm://<your-windows-machine>.classroom.puppet.com --user Administrator --no-ssl --password```

**_You will be prompted for a password._**

The output will look similar to the following:

```
STDOUT:

   Status   Name               DisplayName
   ------   ----               -----------
   Running  w32time            Windows Time

Successful on 1 node: winrm://<your-windows-machine>.classroom.puppet.com
```

### Run remote scripts

Sometimes you have scripts for doing some task, but you do not have a good way to execute those script on a remote machine - or many machines simultaneously. Bolt can do that.

In this exercise you will run a script against your remote host. Regardless of platform, the script:

* Sets up a simple MOTD (message of the day).
* Deploys and configures NTP.
* Prints a simple inventory report.

**_For simplicity, the scripts have been pre-loaded onto your student machines. You can see them at `/tools`._**

#### Use Bolt on a Linux machine to run a script on a Windows machine

On your Linux machine, run the following in a terminal window:

```$ bolt script run /tools/windows.ps1 --targets winrm://<your-windows-machine>.classroom.puppet.com --user Administrator --no-ssl --password```

The output will look similar to the following:

```
STDOUT:
   Updating the MOTD Info...
   MOTD Set successfully!
   Configure Time server...
   The command completed successfully.
   Sending resync command to local computer
   The command completed successfully.
   Time server set correctly!

   Inventory report:

   Hostname               IPAddress     OSName                                   DiskUsed
   --------               ---------     ------                                   --------
   <your-windows-machine> 172.31.44.123 Microsoft Windows Server 2016 Datacenter    49.41

Successful on 1 node: winrm://<your-windows-machine>.classroom.puppet.com
Ran on 1 node in 6.08 seconds
```

# Discussion questions

* How might you use Bolt for simple ad hoc administration tasks?
* What could you do if you want to run against a large group of hosts instead of a single target?
* How might being able to run existing scripts help you enable other teams?

Next Step
======

[Windows Lab 2.2 Running Bolt Commands](../../Windows/lab-2.2-Running-Bolt-Commands)
---

|  [Next Lab](../lab-5.1-Puppet-Agent-deployment)  |  [Previous Lab](../lab-2.1-Install-Puppet-Bolt)  |