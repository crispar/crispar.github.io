---
published: true
title: Daily Linux Study
layout: post
author: jungwook
category: Linux
tags:
- Linux
---

### Rapidly invoke an editor to write a long, complex, or tricky command

Next time you are using your shell and need to enter a long command, try `ctrl-x e` (press `x` and `e` while holding control key).

The shell will take what you\`ve written on the command line so far and paste it into the editor specified by `$EDITOR`. Then you can edit at leisure using all the powerful macros and commands of `vi`, `emacs`, `nano` or your preferred editor.

> You can set a another editor as below :
> `export EDITOR=nano`
> `export EDITOR=vi`

### Breaking out of a terminal when ssh locks

At times an `ssh` connection times-out or disconnects and the current terminal locks.

Normal key combinations are forwarded over the ssh session, therefore no standard breaking keys will work.

Instead, use the escape sequences. To kill the current session hit `↵` and write `~`.

More of these escape sequences can be listed with `↵`
and `~?`:

```
Supported escape sequences:
~. - terminate session
~B - send a BREAK to the remote system
~R - Request rekey ( SSH protocol 2 only)
~# - list forwarded connections
~? - this message
~~ - send the escape character
```

Note. `↵` is the Enter key.


### Random password generator

you can generate passwords in the terminal in many ways. Two of them are listed below.

Using `openssl`:

```{.bash}
$ openssl rand 32 -base64
PKjZhfmUYdHoNWQuAQVTY8smpPOfMXmY/xQWf8A1d+w=
$
```

The `rand 32` option tells `openssl` to generate a random sequences of 32 bytes and `-base64` tells `openssl` to base64-encode the output (see the link at the end for details).

Using `pwgen`:

```{.bash}
$ pwgen -s 40
FTdsL3jmSEkPHhTUm0dhMv1V5UeKnsAzneCS7zrO
$
```

By default `pwgen` generates pronounceable passwords. The `-s` option tells `pwgen` to generate completely random, secure password instead (containing only letters and numbers). `40` is the length of password to generate.

By using the `-y` option we can tell `pwgen` to also include symbols in the password:

```{.bash}
$ pwgen -s -y 40
1AvI$|o"KT|p}1WkNl`an<MV~t&"R!9R~W!pY,&@
$
```

### Keep useful commands in your shell history with tags

Tag commands for future reference:

```{.bash}
$ some_command -some_flag some-path # useful
```

Then it is possible to search your bash history using

`CTRL+R`

```{.bash}
(reverse-i-search)`#useful`: some_command -some_flag some-path # useful
```

Because `# useful` is just a comment in bash you are not limited to this word. Therefore, creative use of tags can save you time.

Command history is saved in ~/.bash_history file, which is emptied after a history limit is reached. In order to preserve the tag collection, a backup of the file is needed.

### Get to know your commands with type

The `type` command is used to differentiate between *builtin* commands and *exteranl binary*

Subsequently, you can find different types of commands[^1]

Find an **alias**:

```{.bash}
$ type ls
ls is aliased to `ls --color=auto'
```

Find a **file**:

```{.bash}
$ type rm
rm is /bin/rm
```

Find a **builtin**:

```{.bash}
$ type cd
cd is a shell builtin
```

Find a **function**:

```{.bash}
$ type psgrep
psgrep is a function
psgrep()
{
  ps -ef | {
    read -r;
    echo "$REPLY";
    grep --color=auto "$@"
  }
}
```

you can also check the type of `type`:

```{.bash}
$ type type
type is a shell builtin
```

---

[^1]:INFO
`type` can find whether the targeted command is an alias, a keyworkd, a function, a builtin or a file.
