# Lab 8.1: Create a wrapper module

In this lab you will learn how to:

* Use PDK to create a cross-platform module to configure the time server.
* Configure the `time` class.
* Add your module to version control.
* Publish your module on the classroom version control server.

# Steps

### Create the module structure

In this task, you will create the `time` module with a single `time` class. The file that contains this class is named `init.pp` because the module that it contains is named `time`. This allows Puppet to automatically load this class by searching the [modulepath](https://puppet.com/docs/puppet/latest/dirs_modulepath.html) for its name when you declare it.

On Windows you have 2 options, Visual Studio Code (VSCode) or PowerShell, you could also use a combination of the two. For simplicity we will use VS Code's Terminal to run commands and edit our code all in one window.

1. Open VSCode
1. Select `Terminal` then `New Terminal`
1. In the terminal window of VSCode run this command:

    ```pdk new module```

1. You will see several questions requiring an answer. Enter these answers:

**_Replace the “N” in these answers with your student number (for example, `student8`)._**

| Question           | Answer            |
| ------------------ |-------------------|
| **Module Name**        | *time*              |
| **Forge Name**         | *studentN*          |
| **Credit Author**      | *Student N*         |
| **License**            | *Apache-2.0*        |
| **Operating Systems**  | *RedHat and Windows*|

* Press `Y` to generate the module in the current directory.

* Change the current working directory to the module you created, then use the `pdk` command to create a new class called `time`.

```
PS C:\Users\Administrator> cd time
PS C:\Users\Administrator\time> pdk new class time
```

The output should show the names of files that the last command created:

```
pdk (INFO): Creating 'C:/Users/Administrator/time/manifests/init.pp' from template.
pdk (INFO): Creating 'C:/Users/Administrator/time/spec/classes/time_spec.rb' from template.
```

### Add classes to your wrapper class

In the Visual Studio Code application add resources to the `time` class.

1. Choose **File > Open File**.
1. Change directory into the `time` module directory.
1. Change directory into the `manifests` directory.
1. Open the `init.pp` file.
1. Use the code below to do the following:
    * Declare the classes `winntp` and `ntp`.
    * Both of these two Forge modules’ main classes have a parameter `servers`. Use a variable named `$servers` to pass the array `['time.google.com']` to both classes.
    * Create a case statement that uses the `$facts['kernel']` fact to apply the `winntp` class to Windows and apply the `ntp` class as the default selection.

        ```ruby
        class time {  
          $servers = ['time.google.com']
            
          case $facts['kernel'] {
            'windows': {
              class { 'winntp':
                servers => $servers,
              }
            }
            default: {
              class { 'ntp':
                servers => $servers,
              }
            }
          }
        }
        ```

### Track your module with version control

It’s important to add your module’s files to version control so you can track changes. You will use Git as your version control software. 

Before you can use Git, you need to configure it. These steps are the same for Windows and Linux:

```
$ git config --global user.email "noreply@puppet.com"
$ git config --global user.name "studentN"
```

#### Now commit the code of your newly created module and class, as described below.

1. Open Visual Studio Code.
1. Choose **File > Open Folder**
1. Change directories back up into the main `time` directory and click **Select Folder** to open it.
1. Click the Source Control icon on the left of the window (third down, below the magnifying glass icon).
1. Click the small diamond icon in the upper left of the pane (its tool tip says **Initialize Repository**).
1. Click **Initialize Repository** in the resulting dialog.
1. Click the ellipsis where the small diamond icon was in the upper left of the pane.
1. Choose **Stage All Changes**.
1. Using the ellipsis choose **Commit Staged**.
1. Enter `Initial Commit` into the commit message dialog and press `Enter`.

**Changes** should now be displayed as **(0)**.

### Publish your module

* To share your module with colleagues and Puppet masters, push it to a remote Git server. Running the following command from the base directory of your time module adds the URL to push to using the name `origin`.

    ```PS C:\Users\Administrator\time> git remote add origin git@gitlab.classroom.puppet.com:puppet/time.git```

* To keep your module changes from affecting other users in the class, create a branch named after your student number.
* Still in the Terminal of VSCode

    ```PS C:\Users\Administrator\time> git checkout -b studentN```

* Push the branch to the remote Git server:

1. Open Visual Studio Code.
1. Click the Source Control icon on the left of the window (third down, below the magnifying glass icon).
1. Click the ellipsis where the small diamond icon was in the upper left of the pane.
1. Choose **Publish branch**.
1. Choose **No** in the bottom right dialog about periodically running `git fetch`.

Whether you are on Windows or Linux, you can inspect the files you just committed by pointing a web browser to https://classXXXX-gitlab.classroom.puppet.com/puppet/time/tree/studentN

**_If prompted to sign-in, use the username *studentN* and the password *puppetlabs*_**

## ***THIS IS OPTIONAL***

### Create the module on the other host

**_Only complete this section if you would like to work on both hosts._**

Optionally, add the time module to the platform you did not use for the earlier part of this lab.

##### If you created the module on Windows and want to clone it to Linux

As the `centos` user on Linux run:

```        
$ git clone git@gitlab.classroom.puppet.com:puppet/time.git
$ git fetch origin studentN
$ git checkout studentN
```

# Discussion questions

1. Is using PDK required when you create a new module? If not, what advantages do you get from using PDK?
1. What problems do you solve by using a version control system like Git?

|  [Previous Lab](../lab-7.1-Puppet-Forge)  |  [Next Lab](../lab-9.1-Test-module-syntax-and-style)  |