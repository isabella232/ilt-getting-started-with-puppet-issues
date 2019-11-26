# Lab 10.1: Create roles and profiles

In this lab you will learn how to:

* Clone a Puppet Enterprise control repository.
* Refactor the time module into a profile.
* Add the time profile to the base profile.
* Create a bastion role and add the base profile to it.

**_Whenever you see_** **classXXXX,** **_ it refers to your class ID, such as_** **class1809.**

**_Whenever you see_** **studentN,** **_ it refers to your assigned student number, such as_** **student4.**

# Steps

### Clone the control repository

In this task, you will clone the control repository to your Windows workstation.

1. Open Visual Studio Code.
1. Click **View**.
1. Click **Command Palette**.
1. In the box that opens, start typing *git clone* and select **Git: Clone** when it appears.
1. In the **Repository URL** field, enter *git@gitlab.classroom.puppet.com:puppet/control-repo.git* and press `Enter`.
1. In the dialog box that opens, click **Select Repository Location**.
1. You will be prompted in the lower right corner to **Open Repository**. Click the button.
1. Now that you have successfully cloned the repository, switch to your student branch:
  1. Click the branch icon followed by the word **production**, in the lower left corner.
  1. In the **Select a ref to checkout** dialog list, select **origin/studentN** where N is your student ID.

### Create a base profile

With your control repository cloned locally, you are ready to start updating Puppet code. In this task, you will take the **time** class you created in the previous lab and declare it in a profile.

Time is a perfect module to add to the `base` profile: that is, a profile that should be configured on every workstation. In this simple profile, you are going to include your time class. In this way, you will ensure that all systems apply the `base` profile, and included in the `base` profile is the time class.

1. In the `control-repo/site/profile/manifests` directory, create a new file called `base.pp`. It will open in the Visual Studio Code window on the right. 
1. Update `base.pp` to contain these contents:

```ruby
  class profile::base {
    include time
  }
```

### Convert the module using the Pupppet Developer Kit

1. Change directories to `C:\Users\Administrator\control-repo`
1. Run the PDK convert command

    ```C:\Users\Administrator\control-repo> pdk convert```

1. Accept `Y` when asked `Do you want to continue and make these changes to your module?`

### Validate and test the syntax

1. Test the syntax. From your shell window run:

    ```PS C:\Users\Administrator\control-repo> pdk validate```

Now when you apply the `base` profile to all workstations, they will have NTP configured by default.

### Add the time module to the control repo branch

1. In Visual Studio Code, open the file named `Puppetfile` in the control repository.
1. In `Puppetfile`, add a module (`mod`) entry for the Git remote you pushed your module to previously. Remember to set the `branch` key to your correct student number.

    ```ruby
      mod 'time',
        :git    => 'git@gitlab.classroom.puppet.com:puppet/time.git',
        :branch => 'studentN'
    ```

### Create bastion host role

Now that you have a `base` profile, increase your level of abstraction by placing that `base` profile in a **role**. Roles are used to classify node types in Puppet Enterprise Console.

For this example, you will ensure all bastion hosts have the `base` profile applied. It applies the class `ntp` or `winntp` to Linux or Windows respectively.

1. To add the new role in Visual Studio Code, navigate from **site** to **role** to **manifests**.

    ```
    site
    |_role
      |_manifests
    ```

1. Create a new file called `bastion.pp` in the `manifests` directory.

1. To configure the `bastion` role, enter the following code into the empty `bastion.pp` file:

    ```ruby
    class role::bastion {
      include profile::base
    }
    ```

1. Test the syntax by running:

    ```PS C:\Users\Administrator\control-repo> pdk validate```

1. If any syntax errors are detected, fix them and re-run the PDK Validate tool.

## Apply the `bastion` role to your workstation

Now that you have tested your profile and role, it is time to apply the role to your workstation. 

In this lab, you will test it with `puppet apply`. In a future lab, you will see how to classify your node in the Puppet Enterprise master.

Create an `examples` directory where you can place files to test your profile:

1. In Visual Studio Code, right-click **site** and then **role** and select **New Folder** and name it `examples`
1. Inside the `examples` folder, add a file called `bastion.pp`. Your file system structure should look like this:

    ```
    site
    |_role
      |_examples
        |_bastion.pp
    ```

1. Add these contents into `bastion.pp`:

    ```ruby
    include role::bastion
    ```

1. Save `bastion.pp`.

1. Now you can smoke test this class and then apply it locally. At the terminal window that opens, change directory into your `control-repo\site\role` directory. First run a smoke test (noop) and then `puppet apply` the role, using these platform-specific instructions:

#### Smoke test your code

1. Click the Windows Start button in the bottom left corner of the workstation and scroll down to **Puppet**. In that folder, open the program **Start Command Prompt with Puppet**.

1. In the terminal window that opens, run:

    ```
    PS C:\Users\Administrator> cd c:\Users\Administrator\control-repo\site\role
    
    # This is the smoke test. --noop is a dry-run.
    PS C:\Users\Administrator> puppet apply examples\bastion.pp --noop --modulepath='C:/ProgramData/PuppetLabs/code/environments/production/modules;C:/Users/Administrator/control-repo/site;C:/Users/Administrator'
    
    # Apply it for real
    PS C:\Users\Administrator> puppet apply examples\bastion.pp --modulepath='C:/ProgramData/PuppetLabs/code/environments/production/modules;C:/Users/Administrator/control-repo/site;C:/Users/Administrator'
    ```

1. Verify Puppet actually changed your time server to `time.google.com` by running this command. Your output should be similar.

    ```
    PS C:\Users\Administrator> w32tm /query /peers
    #Peers: 1
    
    Peer: time.google.com,0x9
    State: Active
    ```

### Commit your branch to Git

1. In Visual Studio Code, click the source control icon on the left menu. It will ask for a commit message. Enter `Adding Bastion Role and Base Profile`.
1. Next to each of your additional and modified files, click **+** to stage the changes. 
1. Click the check mark at the top to commit the changes. 
1. Click the ellipsis in the ***Source Control:Git** menu and select **Push**.

In the next lab, you will expand on the base profile.

# Discussion questions

1. How do roles and profiles differ?
1. At your job, what are some roles that could be useful?

|  [Next Lab](../lab-12.1-Expand-initial-roles-and-profiles)  |  [Previous Lab](../lab-9.1-Test-module-syntax-and-style)  |