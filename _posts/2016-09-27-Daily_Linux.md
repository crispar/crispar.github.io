---
published: true
title: Daily Linux Study
layout: post
author: jungwook
category: Linux
tags:
- Linux
---

### Excuting commands with sudo without password


To `sudo` without password, you can:
+ set the `sudoers` group to excute commands without being promoted
+ set it up only for our user

Call `visudo` to start editing the `/etc/sudoers` file and set to enable the feature for the group.

```
%sudo ALL=NOPASSWD: ALL
```

Alternatevely, to enable it only for yourself:

```
YourUserName ALL=(ALL) NOPASSWD: ALL
```

Note: This is highly insecure and you shoud avoid doing it.


### The setuid permission

When set-user identification (`setuid`) permission is set on an executable file, a process that runs this file is granted access based on the owner ot the file, rather than the user who is running the executable file.

This special permission allows a user to access files and directories that are normally only available to the owner. For example, the `setuid` permission on the `passwd`command makes it possible for user to change its password, assuming the permissions of the root:

```
$ ll /usr/bin/password
-rwsr-xr-x 1 root /usr/bin/passwd*
```

The `s` in the permissions shows that special permissons is set.

We can set this special permissions with:

```
$ chmod +s file
```

### Running a command as another local user

To execute a command as a different user, call **sudo** with the **-u** argument:

```{.bash}
$ sudo -u user command
```

Where user is the name of the user you want to execute the command as.

Example:
```{.bash}
foo@linux:~$ whoami
foo

foo@linux:~$ sudo -u test whoami test
foo
```

### Granting root access to a user

To give a specific user `root` access, you need to add the user to the `root` permissions group.

if you want to create a new user:

```{.bash}
$ adduser newUser
$ passwd newUser
```

To add newUser to the *root* group, run:

```{.bash}
$ adduser newUser root
# or
$ usermod -aG root newUser
```

Or to give it root previleges manually:

```{.bash}
$ visudo
# and add the following line under
newUser ALL=(ALL:ALL) ALL

# User privilege specification
root ALL=(ALL:ALL) ALL
newUser ALL=(ALL:ALL) ALL
```

Note that the user must log off and back on for this change to take effect.

### Run previous command as root

Without including `sudo` before a command, sometimes a 'permission denied' message can be received.

`sudo !!` solves this problem.

Upon entering the following:

```{.bash}
$ rm -r private_file
```

You won't have the permission to execute this unless you're logged in with elevated privileges.

What `sudo !!` does is it runs the previous command as `root`. Meaning it now becomes :

```{.bash}
$ sudo rm -r private_file
```


