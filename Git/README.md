# Git tutorial

Git is a Version Control System that allows users to collaborate on code and have a complete history of their work in centralized location.

The purpose of this tutorial is not to give you a full and complete understanding of Git but instead, this is designed to give you the necessary knowledge to be successful in class. In class we use a subset of git commands that will be shown here. Please use this as a reference for class, your company may and probably has a standard way they implement and use git.

### Configure

When you first start using Git, you need to setup your user information. To do this you can set two `--global` options like this...

   ```git config --global user.name "[your name]"```

   ```git config --global user.email "[your email address]"```

**_This needs to be done on a per user, in other words, if you do this a the `centos` user and then sudo to `root`, you would also have to setup the same config for your `root` user._**

### Creating repositories

Creating a repository is quite easy. When starting out in a local directory you have created you can initialize the directory.

   ```git init```

Now that our local directory is initialized, we need to setup the link to the remote system, this tells our local repo where the remote is located and gives it a common name.

   ```git remote add origin git@gitlab.classroom.puppet.com:puppet/time.git```

   **_With a repository there are 2 methods that can be used for the URL, SSH or HTTPS, the above example is using the SSH method. For class we will be using the SSH method._**

### Cloning repositories

The other way you can have a repository is to clone it. This is used when the repo was already created by someone else and is in remote system. To clone the repo you would simply run

   ```git clone [url]```

    Example: `git clone git@gitlab.classroom.puppet.com:puppet/control-repo.git`

   **_With cloning a repository there are 2 methods that can be used, SSH or HTTPS, the above example is using the SSH method. For class we will be using the SSH method._**

### Synchronizing changes

Before you make any changes to a local repository, you want to ensure you have the latest version. You might not be the only one working on a specific git repo and as such, you need to ensure you are not working on an outdated set of code. To do this, you would need to sync your local repo to the remote.
You can use 2 different command either, in class we will use `git pull`

   ```git pull```

   or

   ```git fetch```

### Branches

Branches are a way to copy all existing code into a `new` branch to work in without affecting the main branch, typically called `master`. By using branches, this allows multiple users to edit the same code without stepping on each others toes. Branches can be used to make new features, fix bugs etc.

To create a new branch you simply run

   ```git checkout -b [branch name]```

You can also list all the current branches using

   ```git branch```

Or you can even list all branches including those on the remote using

   ```git branch -r```

To checkout or switch to an existing branch you would use

   ```git checkout [branch name]```

### Making Changes

Now that you have a local repository, now comes the fun part, editing local files. Remember the first thing you always want to do before editing is to run a `git pull` and ensure you have the latest version. Then using your favorite editor, commence to wreaking havoc on your code. Once you are done, you now have to get your code changes to sync with the remote so that others can see your code as well. For this we will use the `git push` command.

First, though we need to understand how does Git know what to track in version control. This is where the stage comes into play. The stage, similar to the stage actors use, is where we put our files for Git to know about. Think of it this way, our local files are back stage, never seen by the public, however when we want the public to see them we have to put them out in front of them, hence staging our files. Within Git we use the `git add`

   ```git add --all```
   
   ```git add -A```
   
   ```git add .```
   
   ```git add *```

All of these do the pretty much the same thing, take anything that is in the local directory and add the files to the stage. Side note teh `--all` and `-A` are the same thing. 

Now that our files are on the stage, we need to commit them to the index and to do that we do a `git commit` and add a message. This message is to let others know the changes we made and why. You can put anything you want here, be as detailed or a vague as you want to be. 

   ```git commit -m [message]```

If you leave off the `-m [message]` part, git will open an editor for you to type in a much longer message instead of trying to type a single line entry.

   ```git commit```

Now that we have our commit message we are ready to send our code to the remote system using the `git push` command.

   ```git push```