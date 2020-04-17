# Lab 13.1: Class params

In this lab, you will learn how to:

* Add class parameters to a profile.
* Configure nodes using class parameters in the PE console.
* Configure nodes using configuration data in the PE console.

As you have learned, roles and profiles are important constructs that help you minimize rework and ensure that you have common implementations of various technologies on multiple different nodes.

In this lesson, you will learn how to customize your shared profiles so they are truly reusable for all conditions. Specifically, you will update the time profile you created in an earlier lab. You will reclassify your nodes several times to show all the ways you can manipulate data from within the Puppet Enterprise console.

**_Whenever you see_** **classXXXX,** **_it refers to your class ID, such as_** **class1809.**

**_Whenever you see_** **studentN,** **_it refers to your assigned student number, such as_** **student4.**

## Setup

To complete this lab, you will need either a Windows host machine using Visual Studio Code, or a Linux shell.

## Steps

### Update the time module

In this step you will make a slight change to the existing code in your `time` module, specifically the `time/manifests/init.pp` file.

Connect to your Windows student machine and open Visual Studio Code with your cloned time repo. Or if you are on Linux, open the appropriate file with an editor.

Change the code from this:

```ruby
class time {
  $servers = ['time.google.com']
```

To this:

```ruby
class time (
  Optional[Array[String]] $servers = undef,
) {
```

What did you just do? For one, you deleted the static values used in a previous version of this class: `$servers = ['time.google.com']`. Also, you updated the class to use a class parameter (line 2 of the new version). Line 2 defines the `$servers` parameter, which is undefined by default. This means that if no value is passed for this parameter, the default time servers in class `winntp` and `ntp` will be used.

### Commit your code, and push it to a remote repo

1. In Visual Studio Code, click the Source Control (fork) icon on the left.
1. Select the ellipsis to the right of **SOURCE CONTROL: GIT** and click **Stage All Changes**.
1. Select the ellipsis again and click **Commit Staged**.
1. Enter your commit message and press `Enter`.
1. Click the ellipsis again and click **Push**.

## Update the code that configures the time servers

Now that we have updated the time profile, we will use the new Puppet code to update how the time servers get configured on managed nodes.

1. Open the Puppet Enterprise console.
1. In classification, navigate to your **studentN-hosts** node group.
1. In the top right corner of the **studentN-hosts** node group interface, click **Run**. Click **Puppet**.
1. In the **Run Puppet** UI, click **Run job** in the lower right hand corner.

Notice that the Puppet run has made changes, but you did not update anything. However, you did remove the static servers list, so the modules `ntp` and `winntp` now use their default values.

## Configure the time server on your managed nodes

For this step, you will set the values of the time servers in the Puppet Enterprise console. Before you can do that, you have to make some changes to your classification. Right now you are using a role to classify your hosts. Before you see how to adjust data values of a profile included in a role, you will first do so for a profile that is directly classified.

1. Change the classification rules:
    1. Open the Puppet Enterprise console.
    1. In classification, navigate to **studentN-hosts**.
1. Add your parameter:
    * Scroll down to the **Data** section at the bottom of the **Configuration** tab.
    * Populate the **Class** field with *time*
    * Populate the **Parameter** field with *servers*
    * Populate the **Value** field with *["time.google.com","time1.google.com"]*
  
    Remember that you set up your time profile to accept an array? The Puppet Enterprise console also allows you to supply structured data in JSON format (as you are doing in this step). You must enter any structured data in JSON format or it will be converted to a string. Be sure to use double quotes around the values inside the JSON array.
    * Click **Add data** on the right side.
    * Click **Commit ... changes** in the lower right corner.
1. Run Puppet:
    1. Navigate to the **studentN-hosts** group in the Puppet Enterprise console.
    1. In the top right corner, click **Run** and select **Puppet**.
    1. Click **Run job**.
    1. You will see that both hosts have made changes. Click their respective **Report** links to view the details.

## Discussion questions

1. What is the advantage of using parameters to configure services on your nodes, instead of hard-coded strings?
1. Are there any disadvantages to using parameters?

|  [Previous Lab](../lab-12.1-Expand-initial-roles-and-profiles)  |  [Next Lab](../lab-14.1-Deploy-an-application)  |
