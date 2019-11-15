# Lab 14.1: Deploy an application

In this lab you will learn how to create a role and profile structure for deploying a Java app.

* Set up the required directory structure.
* Install Java.
* Copy the application JAR into place.
* Configure the JAR to run as a system service.
* Work for both Windows and Linux.

The labs completed thus far have primarily been focused on operating system configurations, but you can do much more than that with Puppet. In this lab, you will build a cross platform role that can deploy a simple Java application.

## Steps

### Create the new role and supporting profile

1. Open Visual Studio Code.
1. Within your control repo, navigate to `site/role/manifests` and create a new file called `sample_app.pp`
1. Enter the following contents into the new file:

    ```ruby
    class role::sample_app {
      include profile::base
      include profile::sample_app
    }
    ```

1. Create the supporting `profile::sample_app` profile by navigating to `site/profile/manifests` within the control repo and creating a file called `sample_app.pp`
1. Enter the following contents into the new file:
  
    ```ruby
    class profile::sample_app {
      case $facts['kernel'] {
        'windows': {
          include profile::sample_app::windows
        }
        'Linux':   {
          include profile::sample_app::linux
        }
        default: {
          fail('Unsupported operating system!')
        }
      }
    }
    ```

**_You have just added one more level of abstraction: you have created different profiles depending on whether this is a Windows or Linux host. This lets us use one top-level role that can be applied regardless of platform (similar to the `role::bastion` that you used before)._**

### Create the operating system-specific profiles

1. Within the `site/profile/manifests` folder, create a new folder called `sample_app`
1. The contents of the Linux and Windows manifests can be found in the **Downloads** page on the presentation for this module. Download `lab14_linux.pp` and `lab14_windows.pp` to your student machine.
1. Copy `lab14_linux.pp` to the `site/profile/manifests/sample_app` folder in your control repo, and rename the file to `linux.pp`
1. Copy `lab14_windows.pp` to the `site/profile/manifests/sample_app` folder in your control repo, and rename the file to `windows.pp`
1. Copy your application JAR file into place, using the instructions that follow.
    1. Create a new folder called `files` inside of the `site/profile` directory. The JAR will live here.
    1. Normally, application artifacts such as JARs would be served from an artifact repository, HTTP server, or similar file share and not from a module. But to keep things simple for this lab, download the JAR from the **Downloads** page on the presentation for this module (the file is called `puppet_webapp.jar`).
    1. Copy the JAR file into the `files` directory you just created.
1. Inspect the manifests. For both platforms you will see that the profile does several things:
    1. Creates a directory structure for installing the application.
    1. Copies the required JAR from the Puppet master file server.
    1. Sets up logging.
    1. Creates a service definition and starts it.
1. Commit and push your code updates:
    1. In Visual Studio Code, click the Source Control (fork) icon on the left.
    1. Select **SOURCE CONTROL: GIT** (and click **yes** if prompted).
    1. Type in your commit message and press Enter.
    1. Click the ellipsis to the right of **SOURCE CONTROL: GIT** and click **Push**.

### Update classification

Now that you have your new role and profiles ready, you need to update your classification.

1. Open the PE console:
1. Navigate to **Classification** and click the **studentN-host** group.
1. On the **Configuration** tab, remove all classes:
    1. Click **Remove all classes**.
    1. Click **Commit ... Changes** in the lower right corner.
1. Add your new role:
    1. Click in the **Add new class** input field and type/select `role::sample_app`
    1. Click **Add class**.
    1. Click **Commit 1 change** in the lower right corner.

### Run Puppet

Now run your code and see the application get deployed.

1. In the **studentN-hosts** node group, click **Run** in the top right corner and select **Puppet** from the drop down.
1. In the **Run Puppet** UI, click **Run job** in the lower right corner.
1. Inspect the agent reports to view the changes.
1. Validate that the application is running properly:
    1. On Windows, open a web browser and navigate to `http://localhost:9999`

# Discussion questions

1. What do you think happens if a managed node is missing some dependency when Puppet tries to apply a role or profile (for example, if the Java environment is not installed)?
1. Can you summarize the difference between a role and a profile?