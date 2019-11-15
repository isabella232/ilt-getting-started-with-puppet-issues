# Lab 5.1: Agent deployment

In this lab you will learn how to:

* Install the Puppet agent.
* Connect to your agent node using RDP.
* Identify the pre-installed tools and understand their purpose.
* Install the Puppet agent from the command line.

# Steps

## Windows

### Validate and configure the agent environment

1. Launch Microsoft Visual Studio Code. You will use this editor to write Puppet code.
    * Click the Windows button in the lower left
    * Click the **Visual Studio Code** folder
    * Click **Visual Studio Code**
1. Open a Windows PowerShell prompt. You will enter commands at the command line here.
    * Click the Windows button in the lower left
    * Click the **Windows PowerShell** folder
    * Click **Windows PowerShell**
1. Verify that Git is installed. You will use this for version control when you edit Puppet code.
    * Switch to the Windows PowerShell window
    * See if Git is installed. If it is installed, the output of this command will show the installed version. Note that your version might be different than what is shown here.

    ```
    PS C:\Users\Administrator> git --version
    git version 2.18.0.windows.1
    ```

### Install the Puppet agent using Bolt

A basic installation script has been pre-staged in your home directory, at `C:\Users\Administrator\install_pe_agent.ps1`. You can review the script by opening it in Visual Studio Code.

1. Install the agent on your local system by using Bolt to run the script in an Administrator PowerShell.

    ```PS C:\Users\Administrator> bolt script run install_pe_agent.ps1 --targets winrm://localhost --user Administrator --no-ssl --password```

1. Verify that the agent installed correctly by navigating to the Start button, then to the Puppet application folder. Click **Start Command prompt with Puppet**. In the new window, run this command. If the output displays a version, the installation was successful.

    ```PS C:\Users\Administrator> puppet agent --version```

# Pause here and run the [Linux](../../Linux/lab-5.1-Puppet-Agent-deployment) Exercise before continuing.

### Sign agent certificates

Now that your agent has been installed, you have to sign the certificates so that the master trusts them and will allow the agents to retrieve configurations.

* Connect to the Puppet Enterprise console (link is in the Welcome page).
*  Log in:
    * **username:** *studentN*
    * **password:** *puppetlabs*
* Navigate to **Unsigned certs** under **SETUP** on the left-hand menu.
* In the **Node name** field, identify your node (using the hostname provided on the Welcome page) and click **Accept** next to the hostname.

**_If you do not see your node in the listing, refresh the page. Also, keep in mind you will see all students machines here - please only accept your own and do not click **Accept All**._**

*  On Windows, open a PowerShell window and run:

    ```PS C:\Users\Administrator> puppet agent -t```

**_If you do not open a new shell you will likely get an error about the command `puppet` not being found. This is because your system path has changed after you opened the shell. Close and open a new PowerShell window to avoid this._**

* After a few seconds of runtime you will see a notice like this:

    ```Notice: Applied catalog in X.XX seconds```

# Discussion questions

* What are some of the benefits to new users of using a graphical development environment?
* You installed the agent non-interactively using a single command. How might this be useful in production environments?

[Next Lab](../lab-6.1-Puppet-resources)