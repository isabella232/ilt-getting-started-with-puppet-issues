# Lab 7.1: Puppet forge

In this lab you will learn how to:

* Navigate and search for modules on the puppet forge
* Display the modulepath where the Puppet master expects modules to be installed.
* Install modules from the Puppet Forge on the Puppet master.
* Classify nodes with installed modules and configure class parameters.
* Run the Puppet agent manually and observe node configuration changes.

# Steps

Follow along with the instructor as he or she demonstrates the capabilities of the Puppet Forge. As the demonstration proceeds, you may perform the same steps in your own web browser.

### Explore the Forge website

Navigate to the Puppet Forge at https://forge.puppet.com and observe the number of modules available for download. New modules are continually added by Puppet, partner companies, and third-party developers. If you develop a module that you think the community could benefit from, the Puppet Forge is the place to house it.

The Forge web site has been constructed to help you quickly find what you need. Notice that you are prompted for something that you want to automate, what type of module (supported or otherwise), what platform support you need, and whether you want a module with tasks included.


### Search for a module

You are using a Linux agent node, enter `ntp` in the search box and click **Search**. 

Notice that a large number of module listings are returned. The Forge web site includes a handy key along the right side of the results page that shows which modules are supported, have tasks, or have other features you might want.

Customize your searches and search results by using the web site’s drop-down menus. For instance, the site allows you to constrain your searches to a specific operating system. Once you receive the search results, you can sort the results in various ways to find the modules that most interest you.

### Find detailed information about a module

Locate the `puppetlabs/ntp` module in the results from the search you performed above. Click into the result to view a page containing more details about the module.

This page includes a great deal of useful information, including supported operating systems, Puppet Enterprise version compatibility, how to add this module to your Puppet master (via a Puppetfile or the `puppet module install` command), and a complete set of documentation. Spend some time exploring the page and its documentation tab.

On the right side of the module’s detail page is a quality score and community rating. You can use these numbers, along with information about how frequently the module is updated and when the last release occurred, to find a module that meets your needs for stability and reliability.

After using the module, you can contribute by filling in the survey questions presented on the page. The Forge web site is designed with the community in mind, helping Puppet developers find what they need and allowing them to rate the modules that are useful to them so others can find those modules as well.


### Learn about the Puppet `modulepath`

Puppet modules are installed on the Puppet master so they are available during catalog compilation. They can also be installed on a workstation for use with the `puppet apply` command during Puppet code development. In either case, they must be installed in a directory that is included in the **modulepath** so the Puppet toolset can find them.

Use the `puppet` tool to see the `modulepath`. The output will include one or more directories, separated by the platform separator character. Your output might differ from what the sample shown here:
 
```
$ sudo puppet config print modulepath
/etc/puppetlabs/code/environments/production/modules:/root/puppetcode/modules:/etc/puppetlabs/code/modules
```

### Install a module and its dependencies

Puppet modules are packaged as compressed `.tar` files. One installation method is shown on the module detail page using the `puppet module install` command.

Some modules have **dependencies**, meaning they rely on code from another module. In this situation, all required modules must be installed to satisfy the dependency requirements. In this lab, we will install the `stdlib` module on our nodes before installing the `ntp` module that requires it.

Copy and paste the module installation commands (shown on each module’s Forge page) into the command line on the node:

```
$ sudo puppet module install puppetlabs/stdlib --version 5.2.0
$ sudo puppet module install puppetlabs/ntp
```

### Verify the installation

Verify the installation of the module (your output might differ):
 
```
$ sudo puppet module list --tree
/etc/puppetlabs/code/environments/production/modules
  └─┬puppetlabs-ntp (v7.2.0)
    └──puppetlabs-stdlib (v4.25.1)
/etc/puppetlabs/code/modules (no modules installed)
```

In the example output above, notice that the `puppetlabs/stdlib` module, which is a dependency of `puppetlabgs-ntp`, is also listed as being installed.

### Make a node group and add a class to that group in the Puppet Enterprise console

The instructor has already installed the modules to the Puppet master, and they are available for classification and can be used to configure agent nodes with the time service on Linux or the Windows platform.

1. Log in to your Puppet Enterprise console account and browse to the **CONFIGURE > Classification** menu
1. We are going to create a new classification group for your hosts and then configure the time service via Puppet.
1. At the top of screen, click the **Add Group** link
    1. Leave **Parent name** set to *All Nodes*
    1. In the **Group Name** field, enter *studentN-hosts* (substituting your proper student number in the name)
    1. Leave **Environment** set to *production*
    1. Leave **Environment group** unchecked
    1. Leave **Description** blank or enter a brief description of this node group
1. Click **Add**
1. Click your node’s classification group
    1. On the **Rules** tab, you need to add your hosts to your node group. At the bottom of the window, there is an option to **Pin specific nodes to the group**. Click the field labeled **Node name**. In the dropdown, select either your Linux or Windows machine and click **Pin node**.
    1. Click **Commit 1 change** in the lower right corner of the screen
1. Select the **Configuration** tab
1. Start typing `ntp` in the **Add new class** text box, and you will see a list of classes that match that string
    1. Select `ntp` from the list
1. Click **Add Class** to add the class to your group
1. Configure a class parameter by clicking **Parameter name** in the **Parameter** box
    1. Select **servers**
    1. Enter *["time.google.com","time1.google.com"]* in the **Value** box
    1. Click **Add parameter**
1. Click **Commit 1 change** at the bottom of the page

### Run the Puppet agent

Trigger a Puppet run on your machine. Observe the output of the command showing the Puppet run and configuration of the time service on the node.

```$ sudo puppet agent -t```
    
Once the run completes, verify the time on the system and ensure that it is accurate. For further verification that the service is configured correctly, open the `/etc/ntp.conf` file to check the list of NTP servers.

# Discussion questions

1. What modules might you find on the Forge, that could be useful at your company?
1. Do you think the operations you performed on the Puppet Enterprise console could have been performed with command line tools instead?

[Next Lab](../lab-8.1-Create-a-wrapper-module)     [Previous Lab](../lab-6.2-Using-and-extending-Facter)