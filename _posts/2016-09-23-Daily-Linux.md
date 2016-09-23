---
published: true
title: Daily Linux Study
layout: post
author: jungwook
category: Linux
tags:
- Linux
---

### Useful aliasing in bash

The `alias` tool is a way to simplify things by giving them a new `"false name"`.

```bash
$ alias short_word="command"
```

you can use an alias instead of longer commands, adding them on your `.bashrc` file to make them permanent.

```bash
$ alias ll="ls -l"
```

In this example, typing `ll` will now return 'long listing format'.

Quick exit with alias:

```bash
$ alias x="exit"
```

Other examples:

```bash
$ alias rm="rm -iv"
$ alias nbrc="nano ~/.bashrc"
```

Now, to open ~/.bashrc in nano text editor.

```bash
$ nbrc
```

Or, to find top10 largest files in your system, you can set the following `alias`:

```bash
alias top10="find . -type f -exec ls -sh {} \; | sort -n -r | head -10"
```

To save the aliases for future use, you have to add the command to the end of `~/.bashrc` file and then execute it:

```bash
#using the alias created earlier
$ nbrc
#add a new aliased command to the end
#of the file then execute ~/.bashrc
$ . ~/.bashrc #there is a point after $
```

### Quick cd tip
The `pushd` command saves the current working directory in memory so it can be returned to at any time.

The `pupd` command returns to the path at the top of the directory stack.

```bash
[/usr/ports]$ pushd /etc
/etc /usr/ports
[/etc]$ pushd
/usr/ports
[/usr/ports]$
```

To list remembered directories use `dirs` command. Or, to undo the last change of directory and go back to last visited path:

```bash
$ cd -
```

### Aliasing ssh connections

When using many ssh connections it is very convenient to alias them.

insted of:

```bash
$ ssh -i ~/.ssh/custom_id_rsa \
root@213.133.109.135 -p 31415
```

Start by editing this file:

```bash
$ nano .ssh/config
```

Add the following:

```bash
Host lisa
IdentityFile ~/.ssh/custom_id_rsa
HostName 213.133.109.135
User root
Port 31415
```

After that, you can ssh by alias:

```bash
$ ssh lisa
```


