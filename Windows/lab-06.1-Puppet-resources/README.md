# Lab 6.1: Puppet resources

In this lab, you will learn how to use the `puppet resource` command to inspect and manipulate Puppet resources. 

In particular, you will:

* Inspect the state of local users and groups.
* Inspect the state of the previously configured time service.

**_Decision time: pick either Windows or Linux, and use the instructions for that operating system for this and all later labs. Do not switch operating systems between labs._**

### Use `puppet resource` to display local users

The `puppet resource` command lists and displays the current state of resources on a target node. It is invoked with a resource type name and optional resource title, and it emits valid Puppet code describing the state of the requested resources.

In a PowerShell window, run these two commands. The output should look similar to the abridged versions included here.

* Get info on all users:

   ```
   PS C:\Users\Administrator> puppet resource user
   user { 'Administrator':
     ensure  => 'present',
     comment => 'Built-in account for administering the computer/domain',
     groups  => ['BUILTIN\Administrators'],
     uid     => 'S-1-5-21-545430610-555444065-152929764-500',
   }
   user { 'DefaultAccount':
     ensure  => 'present',
     comment => 'A user account managed by the system.',
     groups  => ['BUILTIN\System Managed Group'],
     uid     => 'S-1-5-21-545430610-555444065-152929764-503',
   }
   [...]
   ```

* Get info on just the `Administrator` user:

   ```
   PS C:\Users\Administrator> puppet resource user Administrator
   user { 'Administrator':
     ensure  => 'present',
     comment => 'Built-in account for administering the computer/domain',
     groups  => ['BUILTIN\Administrators'],
     uid     => 'S-1-5-21-545430610-555444065-152929764-500',
   }
   ```

### Use `puppet resource` to get information about a group

The `puppet resource` command can display the current state of any resource — as long as Puppet knows about its resource type — on the target node. Similar to displaying user resources in the previous step, the command can also display groups.

In a PowerShell window, run these two commands. The output should look similar to the abridged versions included here.

1. Get info on all groups:

   ```
   PS C:\Users\Administrator> puppet resource group
   [...]
   group { 'Performance Log Users':
     ensure => 'present',
     gid    => 'S-1-5-32-559',
   }
   group { 'Performance Monitor Users':
      ensure => 'present',
      gid    => 'S-1-5-32-558',
   }
   group { 'Power Users':
     ensure => 'present',
     gid    => 'S-1-5-32-547',
   }
   group { 'Print Operators':
     ensure => 'present',
     gid    => 'S-1-5-32-550',
   }
   [...]
   ```

1. Get info on just the group `power users`:

   ```
   PS C:\Users\Administrator> puppet resource group 'power users'
   group { 'power users':
     ensure => 'present',
     gid    => 'S-1-5-32-547',
   }
   ```

### Use `puppet resource` to inspect the WinNTP service configuration

The `puppet resource` command may be used to observe the configuration of the NTP service previously applied to your agent node. 

In a PowerShell window, run this command:

```
PS C:\> puppet resource service W32Time
```

The output should be similar to this:

```
service { 'W32Time':
  ensure => 'running',
  enable => 'true',
}
```

# Discussion questions

* When `puppet resource` describes the state of a resource, it does so by displaying valid Puppet code. How might this be useful?
* What non-Puppet tools could `puppet resource` replace in your workflow?
* Do you think `puppet resource` commands need to be modified to work on different Puppet-supported operating systems?

|  [Previous Lab](../lab-05.1-Puppet-Agent-deployment)  |  [Next Lab](../lab-06.2-Using-and-extending-Facter)  |
