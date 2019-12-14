# Lab 2.2: Running commands

In this lab you will learn how to:

* Use Bolt to run simple commands on a target machine.
* Use Bolt to run existing scripts, written in any language, on a target machine.

## Setup

**_For the remainder of this lab, you will use the instructions for both Windows and [Linux](../../Linux/lab-02.2-Running-Bolt-Commands). Follow both sets of instructions, so you get experience running Bolt on both platforms._**

## Steps

### Use Bolt to run a command on the local machine

You are going to stop the time service on the Windows and Linux machines machine you are connected to, by running a system command on those machines.

#### Use Bolt on a Windows machine to run a command on that machine

Use RDP to connect to the Windows machine you will run Bolt on. Once you are connected, open a PowerShell terminal on that machine, and then run this command within the PowerShell terminal:

```PS C:\Users\Administrator> bolt command run 'net stop w32time' --targets winrm://localhost --user Administrator --no-ssl --password-prompt```

**_You will be prompted for the password._**

**_The `--no-ssl` option allows you to use WinRM over port 5985. However, Bolt supports both SSL and non-SSL WinRM connections._**

The output will be similar to this:

```
STDOUT:
   The Windows Time service is stopping.
   The Windows Time service was stopped successfully.

Successful on 1 node: winrm://localhost
Ran on 1 node in 2.61 seconds
```

# Pause here and run the [Linux](../../Linux/lab-02.2-Running-Bolt-Commands) Exercise before continuing.

### Use Bolt to run remote system commands

Now you will start the time service on a remote machine, which is a bit more interesting.

**_For this exercise you will use your *other* student machine. For example, if you are logged into your Windows machine, you will start the service on your Linux machine._**

#### Start the time service

##### Use Bolt on a Windows machine to run a command on a Linux machine

On your Windows machine, run the following in a PowerShell window:

```PS C:\Users\Administrator> bolt command run 'sudo systemctl start ntpd' --targets ssh://<your-linux-machine>.classroom.puppet.com --private-key C:\keys\private_key.pem --user centos --no-host-key-check```

**_This runs a command on just one remote host. Instead, you could have supplied a list of hosts to run simultaneously - or even an entire inventory file of hosts._**

The output will look similar to the following:

   ```
   Started on <your-linux-machine>.classroom.puppet.com...
   Finished on <your-linux-machine>.classroom.puppet.com:
   Successful on 1 node: ssh://<your-linux-machine>.classroom.puppet.com
   Ran on 1 node in 0.45 seconds
   ```

#### Validate that the time service is running

Run the following in a PowerShell window:

```PS C:\Users\Administrator> bolt command run 'sudo systemctl status ntpd' --targets ssh://<your-linux-machine>.classroom.puppet.com --private-key C:\keys\private_key.pem --user centos --no-host-key-check```

The output will look similar to the following:

```
STDOUT:
   ntpd.service - Network Time Service
   Loaded: loaded (/usr/lib/systemd/system/ntpd.service; enabled; vendor preset: disabled)
   Active: active (running) since Wed 2018-09-05 11:52:28 UTC; 8s ago
   Process: 13756 ExecStart=/usr/sbin/ntpd -u ntp:ntp $OPTIONS (code=exited, status=0/SUCCESS)
   Main PID: 13757 (ntpd)
   CGroup: /system.slice/ntpd.service
           /usr/sbin/ntpd -u ntp:ntp -g

   [...truncated...]
Successful on 1 node: ssh://<your-linux-machine>.classroom.puppet.com
```

### Run remote scripts

Sometimes you have scripts for doing some task, but you do not have a good way to execute those script on a remote machine - or many machines simultaneously. Bolt can do that.

In this exercise you will run a script against your remote host. Regardless of platform, the script:

* Sets up a simple MOTD (message of the day).
* Deploys and configures NTP.
* Prints a simple inventory report.

**_For simplicity, the scripts have been pre-loaded onto your student machines. You can see them at `C:\tools` on Windows._**

#### Use Bolt on a Windows machine to run a script on a Linux machine

On your Windows machine, run the following in a PowerShell window:

```PS C:\Users\Administrator> bolt script run C:\tools\linux.sh --targets ssh://<your-linux-machine>.classroom.puppet.com --private-key C:\keys\private_key.pem --user centos --no-host-key-check --run-as root```

**_Notice the use of `--run-as`. This allows us to handle permission escalations. In this case you are authenticating as `centos` and then changing the user to `root` (via sudo) when the script executes. Your user has no password access to run `sudo`, but if you have a separate `sudo` password you need to supply you can do that as well. See the output of `bolt -h` for details._**

The output will look similar to the following:

```
STDOUT:
   Setting MOTD...
   Completed setting MOTD!
   Configuring NTP....
   Configured NTP!

   Inventory Report:

   Hostname              IPAddress     OSName           DiskUsed
   <your-linux-machine>  172.31.32.23  CentOS-7.5.1804  13%

Successful on 1 node: ssh://<your-linux-machine>.classroom.puppet.com
Ran on 1 node in 2.00 seconds
```

# Discussion questions

* How might you use Bolt for simple ad hoc administration tasks?
* What could you do if you want to run against a large group of hosts instead of a single target?
* How might being able to run existing scripts help you enable other teams?

Next Step
======

[Linux Lab 2.2 Running Bolt Commands](../../Linux/lab-02.2-Running-Bolt-Commands)
---

|  [Previous Lab](../lab-02.1-Install-Puppet-Bolt)  |  [Next Lab](../lab-05.1-Puppet-Agent-deployment)  |
