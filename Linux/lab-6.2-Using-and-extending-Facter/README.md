# Lab 6.2 Using and extending facter

In this lab, you will become familiar with the Facter tool by learning how to:

* Use Facter to display core fact values on a single node.
* Use Facter to display a structured fact.
* Install an external fact script and run it with Facter.
* Display fact values in the Puppet Enterprise console.

# Steps

Facter is invoked on managed nodes during each Puppet agent run. Facter gathers information about the node and sends it to the Puppet master for use during the catalog compilation phase. Typical information gathered by Facter might include IP address, amount of installed memory, kernel version, and other machine-specific information.

Run the following commands on your Windows or Linux classroom agent node, not on your local workstation.

**_Steps 1-4 can be executed from either Windows or Linux. The output will vary, but the commands are the same. Try on both operating systems if you have time to compare the differences._**

### Use Facter to see all facts about a node

Run Facter on an agent node to see a list of all facts it can determine about that node (your output will differ from this abridged sample output):

```
$ facter
  aio_agent_version => 5.5.1
  augeas => {
    version => "1.10.1"
  }
  disks => {
    nvme0n1 => {
      model => "Amazon Elastic Block Store",
      size => "30.00 GiB",
      size_bytes => 32212254720
    }
  }
  dmi => {
    bios => {
      release_date => "10/16/2017",
      vendor => "Amazon EC2",
      version => "1.0"
    },
  is_virtual => true
  [...]
```

### Use Facter to see a single, structured fact about a node

You can also invoke Facter so it will display a single fact instead of the complete set of facts that it knows about.

Notice in the previous sample output, the `is_virtual` fact is a simple string value while others are a nested structure of values. The latter type are called **structured facts**.

View a single, structured fact about the agent node's `os` fact (your output will differ from this abridged sample output):

```
$ facter os
  {
    architecture => "x86_64",
    distro => {
      codename => "Core",
      description => "CentOS Linux release 7.4.1708 (Core)",
      id => "CentOS",
        release => {
          full => "7.4.1708",
          major => "7",
          minor => "4"
        },
      specification => ":core-4.1-amd64:core-4.1-noarch:cxx-4.1-amd64:cxx-4.1-noarch:desktop-4.1-amd64:desktop-4.1-noarch:languages-4.1-amd64:languages-4.1-noarch:printing-4.1-amd64:printing-4.1-noarch"
      },
      family => "RedHat",
      hardware => "x86_64",
      name => "CentOS",
      release => {
        full => "7.4.1708",
    [...]
```

### Use Facter to see a simple fact about a node

Facter also gathers and displays facts that have simple string values.

View a simple string fact about the agent node's `kernel` fact (your output will differ from this sample output if you are on Windows):

```
$ facter kernel
Linux
```

### View facts in the Puppet Enterprise console

So far, you have explored using Facter at the command line. As noted previously, facts gathered by Facter are sent to the Puppet master during catalog compilation. They are then available for display in the Puppet Enterprise console.

Open the console and click the **INSPECT > Nodes** submenu. Clicking a node in the list takes you to a page that displays the list of facts gathered from that node. Notice that the list displays both simple string facts and structured facts.

### Use Facter with an external script

Facter gathers built-in (core) facts that are packaged with Facter. It can also use scripts written by you or a third party to gather custom or external facts. In this exercise, you will install an external fact script that creates a `datacenter` fact. You will then verify that Facter can use the script to retrieve that fact.

First, check that the `datacenter` fact does not already exist. This command should produce no value:

```$ facter datacenter```

Next, move the provided external fact script into place:

1. Download `datacenter.sh` to the Linux machine by visiting the downloads page with your browser, right-clicking the script, and selecting **Copy Link address**. Then run:

  ```$ curl -o datacenter.sh '<pasted location>'```

1. Create the Facter location:

  ```$ sudo mkdir -p /etc/puppetlabs/facter/facts.d```
        
1. Move the script into place where Facter can execute it:

  ```$ sudo mv datacenter.sh /etc/puppetlabs/facter/facts.d```
        
1. Make the script executable:

  ```$ sudo chmod +x /etc/puppetlabs/facter/facts.d/datacenter.sh```
        
1. View the shell script, which echoes a key/value pair and exits:

  ```vi /etc/puppetlabs/facter/facts.d/datacenter.sh```


Finally, verify that the fact has been installed and is accessible by Facter. Notice the new output.

```
$ sudo facter datacenter
pdx
```

# Discussion questions

* Are there any unusual aspects of your company's infrastructure that you might want to capture with external fact scripts?
* Which is easier: remembering the single syntax used by Facter, or the many syntaxes used by different command line tools for collecting the same facts?