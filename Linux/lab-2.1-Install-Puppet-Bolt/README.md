# Lab 2.1: Install Puppet Bolt

In this lab you will learn how to:

* Install the open source Bolt utility on Linux machines.
* Execute a simple command to validate that Bolt is working.

# Setup

You will need three machines:

* Two classroom-provided machines (one Windows, one Linux) to install Bolt onto. These are the **target machines**.
* A Windows or Linux machine that you will use to connect to the target machines. This is the **host machine**.

# Steps

### Installing Bolt onto a **Linux** target machine

**_The steps below explain how to connect to a target Linux machine from your student Windows host machine (used above). If you would rather connect directly from your own laptop, you will have to translate these instructions to the utilities available on your laptop. The private key for the Linux target machine can be downloaded from the Welcome page._**

**_There are many distributions of Linux. This classroom environment uses CentOS, but Bolt supports many different versions. See the official documentation for more details. The guide below is for CentOS 7._**

In this lab you will install Bolt using the built-in package manager of the Linux distribution on your target machine.

#### Connect to the Linux target machine using PuTTY or OpenSSH

**PuTTY method**

1. On your Windows student machine, open a PowerShell window
1. Launch PuTTY:

   ```PS C:\Users\Administrator> putty```

1. In the **Host Name** field, copy and paste the Linux target machine’s hostname from the Welcome page.
1. In the left-hand menu, expand **SSH** and click **Auth**.
1. Under **Private key file for authentication**, click **Browse**.
1. Select the pre-loaded private key from `C:\keys`.
1. Select **private_key.ppk** and click **Open**.
1. Under the **Connection** category, click **Data**.
1. In the **Auto-login username** field, enter *centos*
1. Return to the session view by clicking **Session** on the left-hand menu in PuTTY.
1. In the **Saved Sessions** box, enter *linux_machine* and click **Save** on the right side.
1. Click **Open**. If prompted about the host key not being cached, select **Yes**.

**_For any future connections to this host, you can now simply double-click_** **linux_machine** **_in the_** **Saved Sessions** **_window._**

**OpenSSH method**

1. On your Windows student machine, open a PowerShell window
1. Install openssh using chocolatey:

   ```PS C:\Users\Administrator> choco install openssh```

1. When the install is done, restart PowerShell. You can then use the credentials already on the host.
1. Re-open PowerShell and run:

   ```PS C:\Users\Administrator> ssh centos@<your-linux-machine>.classroom.puppet.com```

#### Install the Bolt package from a repository

On the Linux machine, add the required YUM repository and install the Bolt package:

```$ sudo yum install -y puppet-bolt```

#### Validate the installation

Note that your version might be slightly different.

```
$ bolt --version
1.17.0
```

#### Run Bolt

```$ bolt help```

or simply:

```$ bolt```

This provides a page of helpful information for running Bolt. We will come back to Bolt after setting up the rest of the lab.

Next Steps
======

[Windows Lab](../../Windows/lab-2.1-Install-Puppet-Bolt)
---

| [Next Lab](../lab-2.2-Running-Bolt-Commands) | [Previous Lab](../lab-1.1-Puppet-product-overview) |