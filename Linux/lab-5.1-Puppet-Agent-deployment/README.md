# Lab 5.1: Agent deployment

In this lab you will learn how to:

* Install the Puppet agent.
* Connect to your agent node using SSH for Linux
* Identify the pre-installed tools and understand their purpose.
* Install the Puppet agent from the command line.

We recommend the Windows environment for most students. You will get practice
with Puppet’s recommended module development workflow and utilities. These
are designed to provide a streamlined developer experience that allows you
to quickly start managing infrastructure with Puppet.

Students who are already comfortable working on a Linux command line may use the
Linux Puppet agent instead. If you choose to use this environment, you should know how to
navigate using the Linux command line, and how to use common command line tools
such as vi. We will provide commands for technologies that are introduced in this
course, such as Git and the Puppet agent. In the interest of time, course instructors
will not provide general assistance with the Linux command line during class.

Future labs will use the tools and workflow that are introduced in this module.

# Steps

## Linux

### Validate and configure the agent environment

Verify that Git is installed. You will use this for version control when you edit Puppet code. To do this, run this command in a terminal window. If Git is installed, the output of this command will show the installed version. Note that your version might be different than what is shown here.

```
$ git --version
git version 1.8.3.1
```

### Install the Puppet agent using Bolt

A basic installation script has been pre-staged in your home directory, at `~/install_pe_agent.sh`

1. Install the agent on your local system by using Bolt to run the script in a terminal:

    ```$ bolt script run install_pe_agent.sh --targets localhost```

1. Verify that the agent installed correctly by running this command. If the output displays a version, the installation was successful.

    ```$ puppet --version```

# Pause here and run the [Windows](../../Windows/lab-5.1-Puppet-Agent-deployment) Exercise before continuing.

### Sign agent certificates

Now that your agent has been installed, you have to sign the certificates so that the master trusts them and will allow the agents to retrieve configurations.

* Connect to the Puppet Enterprise console (link is in the Welcome page).
* Log in:
    * **username:** *studentN*
    * **password:** *puppetlabs*
* Navigate to **Unsigned certs** under **SETUP** on the left menu.
* In the **Node name** field, identify your node (using the hostname provided on the Welcome page) and click **Accept** next to the hostname.

**_If you do not see your node in the listing, refresh the page. Also, keep in mind you will see all students machines here - please only accept your own and do not click **Accept All**._**

* On Linux, open a terminal and run:

    ```$ sudo puppet agent -t```

* After a few seconds of runtime you will see a notice like this:

    ```Notice: Applied catalog in X.XX seconds```


# Discussion questions

* What are some of the benefits to new users of using a graphical development environment?
* You installed the agent non-interactively using a single command. How might this be useful in production environments?