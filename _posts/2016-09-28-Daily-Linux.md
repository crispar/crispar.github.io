---
published: true
title: Daily Linux Study
layout: post
author: jungwook
category: Linux
tags:
- Linux
---

### Difference between redirection and pipe

**Pipe** and **redirect** are two similar concepts, yet the distinction between them is made in the following way:
+ `redirect` refer to passing the *output* to a **file** or a **stream**
+ `Pipe` refer to passing the *output* to another **program** or **utility**

Let `p1` be a program that when ran will give a certain output.

To put all the *output* inside a file(say `myFile`), *redirection* is necessary[^1]:


```{.bash}
$ p1 > myFiles
```

However, to use the *output* as *input* of another program(say `p2`) things are different. An auxiliary file can be used (say `temp_file`)[^2]:


```{.bash}
$ p1 > tmp_file && p2 < tmp_file
```

Because this approach is quite clunky, *pipes* have been invited to make things easier. You can achieve the exact same thing with the syntax:


```{.bash}
$ p1 | p2
```

### Run local script remotely

Let\`s say you locally have a script that you want to run on a remote machine.

This can be done by securely copying the file on the server first(`scp`), yet login on the remote machine is necessary.

The `enki.sh` script can be run directly on the remote machine with following command:


```{.bash}
$ ssh user@enki `bash -s` < enki.sh
```

The output of `enki.sh` will be displayed locally

### Monitor the progress of data through a pipe with pv

You can use `pv` to monitor the progress of any pipe. This can be done by installing `pv` and putting it between input / output pipes.

To install:


```{.bash}
$ sudo apt-get install pv
```

Example :


```{.bash}
$ dd if=/dev/urandom | pv | dd of=/dev/null
```

output


```{.bash}
1.74MB 0:00:09 [ 198kB/s ] [ <=> ]
```

You could specify the approximate size with the `--size` if you want a time estimation.

You can also use it to output to stdout:


```{.bash}
$ pv /home/user/bigfile.iso | md5sum
```

output

```{.bash}
50.2MB 0:00:06 [8.66MB/s]
[==>  ] 49% ETA 0:00:06
```

Note that in this case, `pv` recognises the size automatically.

Using `pv` can prove extreamly useful when working with big files or processes taking long time to complete For example you can keep track how fast a file is transferred with `nc` command:


```{.bash}
$ pv myFile | nc -w 1 example.com 3000
```

### SSH pipes

One of the benefits of piping is that you can use it over network and it does wonders for data transfer.

**Compressed file transfer**

You can copy files over the network in an instant. This archives the files into a `tar`  file. compresses it (`tar.gz`) and then copies it to the server:


```{.bash}
$ tar cfz - folder |
  ssh user@enk `tar xfz destdir`
```

Flags:
+ `-c` create a new archive
+ `-z` compresses the archive
+ `-f` takes the files to be archived as argument
+ `-x` extracts the contents

You can use `-C` flag to specify a location in which the file to be extracted:


```{.bash}
$ tar xfzC arch /forder1/folder2
```

**Backing up data with** dd


```{.bash}
$ dd if=/folder/data |
  ssh user@enki `dd of=data.iso`
```

Flags:

+ `if`:input file
+ `of`:output file

Note that half of the command is executed locally, while the other half is excuted remotely.

### Duplicate pipe content with temp_file

To duplicate the content while piping you can use the `tee` utility.

One straighforward and useful example is that `tee` can be used to write to multiple files at the same time.

The command to archive that is:


```{.bash}
$ ps | tee fileone filetwo filethree
```

The output of the `ps` command is now inside three different files.

To append data to files, the `-a` flag must be used:


```{.bash}
$ ps | tee -a fileone filetwo filethree
```

<BR>

[^1]: Explanation

    What happens when running the following line:

    ```{.bash}
    $ p1 myfile
    ```
    1. `p1` is expected
    2. *output* of `p1` is put in a file called `myFile`
      
    If `myFile` already exists, it will be overriden.

<BR>

[^2]: Explanation
 
    What happens when running the line:

    ```{.bash}
    $ p1 tmp_file p2 tmp_file
    ```
    1. `p1` is expected
    2. *output* of `p1` is saved in the file called `tmp_file`
    3. `p2` is excuted having as *input* the contents of `tmp_file`

