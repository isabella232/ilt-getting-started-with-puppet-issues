# Lab 6.1: Puppet resources

In this lab, you will learn how to use the `puppet resource` command to inspect and manipulate Puppet resources. 

In particular, you will:

* Inspect the state of local users and groups.
* Inspect the state of the previously configured time service.

**_Decision time: pick either Windows or Linux, and use the instructions for that operating system for this and all later labs. Do not switch operating systems between labs._**

### Use `puppet resource` to display local users

The `puppet resource` command lists and displays the current state of resources on a target node. It is invoked with a resource type name and optional resource title, and it emits valid Puppet code describing the state of the requested resources.


In a terminal, run these two commands. The output should look similar to the abridged versions included here.

1. Get info on all users:

```
$ sudo puppet resource user
user { 'adm':
  ensure             => 'present',
  comment            => 'adm',
  gid                => 4,
  home               => '/var/adm',
  password           => '*',
  password_max_age   => 99999,
  password_min_age   => 0,
  password_warn_days => 7,
  shell              => '/sbin/nologin',
  uid                => 3,
}
user { 'bin':
  ensure             => 'present',
  [...]
```
    
2. Get info on just the `root` user:

```
$ sudo puppet resource user root
user { 'root':
  ensure             => 'present',
  comment            => 'root',
  gid                => 0,
  home               => '/root',
  password           => '!!',
  password_max_age   => 99999,
  password_min_age   => 0,
  password_warn_days => 7,
  shell              => '/bin/bash',
  uid                => 0,
}
```

### Use `puppet resource` to get information about a group

The `puppet resource` command can display the current state of any resource — as long as Puppet knows about its resource type — on the target node. Similar to displaying user resources in the previous step, the command can also display groups.


In a terminal, run these two commands. The output should look similar to the abridged versions included here.

* Get info on all groups:

```
$ sudo puppet resource group
group { 'adm':
  ensure => 'present',
  gid    => 4,
}
group { 'audio':
  ensure => 'present',
  gid    => 63,
}
group { 'bin':
  ensure => 'present',
  [...]
```

* Get info on just the group `wheel`:

```
$ sudo puppet resource group wheel
group { 'wheel':
  ensure => 'present',
  gid    => 10,
}
```

### Use `puppet resource` to inspect the NTP service configuration

The `puppet resource` command may be used to observe the configuration of the NTP service previously applied to your agent node. 

#### Linux

In a terminal, run this command:

```
$ sudo puppet resource service ntpd
service { 'ntpd':
  ensure => 'running',
  enable => 'true',
}
```

# Discussion questions

* When `puppet resource` describes the state of a resource, it does so by displaying valid Puppet code. How might this be useful?
* What non-Puppet tools could `puppet resource` replace in your workflow?
* Do you think `puppet resource` commands need to be modified to work on different Puppet-supported operating systems?

|  [Previous Lab](../lab-05.1-Puppet-Agent-deployment)  |  [Next Lab](../lab-06.2-Using-and-extending-Facter)  |
