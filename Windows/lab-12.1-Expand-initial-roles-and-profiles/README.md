# Lab 12.1: Expand initial roles and profiles

In this lab you will learn how to:

* Create a profile (`security_baseline`) that contains other profiles (`windows_firewall` and `ssh`).
* Include a profile (`security_baseline`) in another profile (`base`).
* Create roles and profiles in the Puppet Enterprise console.
* Classify nodes and deploy the bastion role.

# Setup

In this lab, you will apply Puppet code locally to make changes, commit the code to GitLab and then push the changes to your workstations via the Puppet master.

To complete this lab you will need a Windows host machine with Visual Studio Code, or a Linux host machine with a shell window.

You will need additional modules for future labs, and now that you are familiar with the `puppet module install` command, you can make sure extra modules are installed. To complete the module installation process, run this command:

### Install additional support modules

```
PS C:\Users\Administrator> puppet module install puppetlabs-registry --version 2.1.0
PS C:\Users\Administrator> puppet module install puppet/windows_firewall --version 2.0.2
```

# Steps

## Create platform-specific security profiles

#### Inspect code for the new security profiles

Starter code to add to create two new security profiles (one for Windows, one for Linux) is already in the control-repo. In this step, you will look at that code.

#### Windows

This Puppet code enables the Windows Firewall and opens three ports: ICMP (ping), WinRM (remote scripting) and RDP (remote console).

In Visual Studio Code, validate that the content of the file `site/profile/manifests/baseline/windows/firewall.pp` is as follows:

```ruby
class profile::baseline::windows::firewall {
  class { '::windows_firewall':
    ensure => 'running',
  }

  windows_firewall::exception { 'Permit ICMPv4':
    ensure       => present,
    direction    => 'in',
    action       => 'allow',
    enabled      => true,
    protocol     => 'ICMPv4',
    display_name => 'Permit ICMPv4',
    description  => 'Permit ICMPv4',
  }

  windows_firewall::exception { 'WINRM':
    ensure       => present,
    direction    => 'in',
    action       => 'allow',
    enabled      => true,
    protocol     => 'TCP',
    local_port   => 5985,
    remote_port  => 'any',
    display_name => 'Windows Remote Management HTTP-In',
    description  => 'Inbound rule for Windows Remote Management via WS-Management. [TCP 5985]',
  }

  windows_firewall::exception { 'RDP':
    ensure       => present,
    direction    => 'in',
    action       => 'allow',
    enabled      => true,
    protocol     => 'TCP',
    local_port   => 3389,
    remote_port  => 'any',
    display_name => 'Windows RDP',
    description  => 'Inbound rule for Windows RDP. [TCP 3389]',
  }
}
```

### Create a `security_baseline` profile

Now you will wrap together the two security profiles you just created into a single new profile called `security_baseline`. As you add additional security modules under each operating system, they can be nested into this `security_baseline` profile for simplified classification.

This profile contains a case statement that takes the `kernel` fact and determines what profiles to apply based on the operating system.

Validate that the content of `site/profile/manifests/security_baseline.pp` as follows:

```ruby
class profile::security_baseline {
  # OS-specific
  case $facts['kernel'] {
    'windows': {
      include profile::baseline::windows::firewall
    }
    'Linux':   {
      include profile::baseline::linux::ssh_config
    }
    default: {
      fail('Unsupported operating system!')
    }
  }
}
```

### Add the `security_baseline` profile to the `base` profile

Since you want security on all workstations, it makes sense to add the `security_baseline` profile to `base.pp`. This ensures that every workstation gets the proper security configuration.

1. Open `site/profile/manifests/base.pp`, and declare the `profile::security_baseline` class:

    ```ruby
    class profile::base {
      include time
      include profile::security_baseline
    }
    ```

2. Save `base.pp`

### Add a smoke test (Optional)

1. Add the example file to allow local `puppet apply` testing:
    1. In Visual Studio Code, right click **site**. Click **profile**. Click **New Folder** and name it `examples`
    1. Inside the `examples` folder, add a file called `security_baseline.pp`

1. Add this to the `security_baseline.pp` file:

    ```ruby
    include profile::security_baseline
    ```

1. Save `security_baseline.pp`
1. Test the new profile:

    ```
    PS C:\Users\Administrator> cd C:\Users\Administrator\control-repo\site
    
    # This is the smoke test.
    PS C:\Users\Administrator> puppet apply profile\examples\security_baseline.pp `
      --modulepath='C:/ProgramData/PuppetLabs/code/environments/production/modules;`
      C:/ProgramData/PuppetLabs/code/modules;.' --noop
    ```

**_It is safe to ignore the yellow *This method is deprecated* warnings for this lab._**

### Commit and push changes

1. In Visual Studio Code, click the source control (Git icon) on the left menu. 
1. When it asks for a commit message, type:

    ```Adding security_baseline profile```

1. Next to each of your additional and modified files, click **+** to stage the changes. Click the check mark at the top to commit the changes, click the ellipsis in the **Source Control:Git** menu, and click **Push**.

## Create Roles and Profiles in the Puppet Enterprise console

This task involves creating roles and profiles similar to those that you made in the control repo, but this time they will be used by the Puppet Enterprise console for the task of classifying nodes.

### Log in to the Puppet Enterprise console

1. Point your browser at https://classXXXX-master.classroom.puppet.com/

1. Enter the credentials:
    * **username:** *studentN*  
    * **password:** *puppetlabs*

1. In the left menu, under **CONFIGURE**, click **Classification**

### Create an environment group

First, create a new node group that has the **environment group** checkbox selected. This node group specifies which nodes to run in your environment (**studentN**), which corresponds to your branch in the control repo.

1. Expand **All Nodes**
1. Click **Add group...** at the top
1. Enter the following:
    * **Parent name:** *All Environments*
    * **Group name:** *studentN-env*
    * **Environment:** *studentN* (where *N* is your student number)
    * **Environment Group:** *checked*
    * **Description:** *This group sets the environment for studentN*
    * Click **Add**
1. Expand the **All Environments** group (click the **+** sign to the left of the name) and click your new **studentN-env** group
1. On the **Rules** tab, navigate to the section that says **Pin specific nodes to the group**
1. Click **Node name** and select only your Linux machine from the list
1. Add your Windows machine from the list
1. Click **Commit 2 changes** in the lower right corner

Next, update the classes assigned in your **studentN-hosts** group:

1. Navigate back to the main classification page
1. Click **studentN-hosts**
1. Update the metadata of this node group to filter classes that belong to the **studentN** environment (your branch):
    * Click **Edit node group metadata**
    * In the Parent dropdown, select **studentN-env** from the list
    * In the Environment dropdown, select **studentN** from the list
    * Click **Commit 1 change** in the lower right portion of the screen
1. Navigate to the **Configuration** tab, click **x Remove all classes**, and click **Commit ... changes** in the lower right corner
1. In the **Add new class** field, start typing `role::` and select `role::bastion` from the list. If you do not see that role in the list click **Refresh** on the right side of the screen to force the console to re-read the code on the PE master.
1. Click **Add class** and **Commit 1 change**
1. Back on the *Rules* tab, ensure both of your hosts are pinned to this group

## Deploy the role

By forcing Puppet to run, you will apply the bastion role to all hosts in your **studentN-hosts** node group.

1. In the left menu, under **RUN**, click **Puppet**
1. In the **Job Description** field, enter *Deploy the bastion role*
1. Under **Inventory** select **Node Group**
1. Under **Choose a node group** select **studentN-hosts**
1. Click **select** and you should see both workstations returned
1. Click **Run job** in the blue bar at the bottom of the screen

This will take a few minutes as Puppet runs and collects data on both nodes. You should see them return **Succeeded** in the Job node status.

**_If you click the date/time stamp under Report, it will give you details of the Puppet run._**

# Discussion questions

1. Why might building up a hierarchy of profiles — instead of just creating a single profile — be useful?
1. Can you think of a hierarchy of profiles that might be useful in your work environment?

[Next Lab](../lab-13.1-Class-params)