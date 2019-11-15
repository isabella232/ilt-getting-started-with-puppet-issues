# Lab 8.1: Create a wrapper module

In this lab you will learn how to:

* Use PDK to create a cross-platform module to configure the time server.
* Configure the `time` class.
* Add your module to version control.
* Publish your module on the classroom version control server.

# Steps

### Create the module structure

In this task, you will create the `time` module with a single `time` class. The file that contains this class is named `init.pp` because the module that it contains is named `time`. This allows Puppet to automatically load this class by searching the [modulepath](https://puppet.com/docs/puppet/latest/dirs_modulepath.html) for its name when you declare it.

* Run this command:

```$ pdk new module```

* You will see several questions requiring an answer. Enter these answers:

**_Replace the “N” in these answers with your student number (for example, `student8`)._**

| Question           | Answer            |
| ------------------ |-------------------|
| **Module Name**        | *time*              |
| **Forge Name**         | *studentN*          |
| **Credit Author**      | *Student N*         |
| **License**            | *Apache-2.0*        |
| **Operating Systems**  | *RedHat and Windows*|

* Press `Y` to generate the module in the current directory.

* Change the current working directory to the module you created, then use the `pdk` command to create a new class called `time`. The commands are the same for Windows and Linux:

```
$ cd time
$ pdk new class time
```

The output should show the names of files that the last command created:

```
pdk (INFO): Creating '/home/centos/time/manifests/init.pp' from template.
pdk (INFO): Creating '/home/centos/time/spec/classes/time_spec.rb' from template.
```

### Add classes to your wrapper class

Open the Visual Studio Code application to add resources to the `time` class.

1. Open the `init.pp` file using the vi editor:
 
    ```$ vi manifests/init.pp```
        
1. Replace the file’s content with the code below. To do this, press `i` to enter insert mode and copy the following content into the file:

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

1. Save the file by pressing `Esc` and typing `:wq` and then `Enter`.

### Track your module with version control

It’s important to add your module’s files to version control so you can track changes. You will use Git as your version control software. 

1. Before you can use Git, you need to configure it. These steps are the same for Windows and Linux:

    ```
    $ git config --global user.email "noreply@puppet.com"
    $ git config --global user.name "studentN"
    ```

1. Now commit the code of your newly created module and class, as described below.

1. If you are not in the base directory of your module, change directories until you are. For example, here is the file structure of your module, and an indication of which directory you want to be in:

    ```
    time  ## YOU WANT TO BE HERE
    ├── appveyor.yml
    ├── CHANGELOG.md
    ├── examples
    ├── files
    ├── Gemfile
    ├── manifests
    │   └── init.pp
    ├── metadata.json
    ├── Rakefile
    ├── README.md
    ├── spec
    │   ├── classes
    │   │   └── time_spec.rb
    │   ├── default_facts.yml
    │   └── spec_helper.rb
    ├── tasks
    └── templates
    ```

1. Run the following:

    ```
    $ git init
    $ git add --all
    $ git commit -m "Initial Commit"
    ```

### Publish your module

* To share your module with colleagues and Puppet masters, push it to a remote Git server. Running the following command from the base directory of your time module adds the URL to push to using the name `origin`. This command is the same on Windows and Linux.

    ```$ git remote add origin git@gitlab.classroom.puppet.com:puppet/time.git```

* To keep your module changes from affecting other users in the class, create a branch named after your student number. This command is the same on Windows and Linux.

    ```$ git checkout -b studentN```

* Push the branch to the remote Git server with a single command:

    ```$ git push origin studentN```

Whether you are on Windows or Linux, you can inspect the files you just committed by pointing a web browser to https://classXXXX-gitlab.classroom.puppet.com/puppet/time/tree/studentN

**_If prompted to sign-in, use the username *studentN* and the password *puppetlabs*_**

## ***THIS IS OPTIONAL***

### Create the module on the other host

Optionally, add the time module to the platform you did not use for the earlier part of this lab.
**_Only complete this section if you would like to work on both hosts._**

##### If you created the module on Linux and want to clone it to Windows

1. In Visual Studio Code, click **View > Command Palette**
1. Type in `git clone` and choose **Git: Clone**
1. Enter the URI `git@gitlab.classroom.puppet.com:puppet/time.git` and press `Enter`
1. In the pop-up window, click **New folder** and name it `time`
1. Click **Select Repository Location**
1. Click **Open Repository** at the bottom left

# Discussion questions

1. Is using PDK required when you create a new module? If not, what advantages do you get from using PDK?
1. What problems do you solve by using a version control system like Git?