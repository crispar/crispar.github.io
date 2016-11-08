---
published: true
title: How to set git prompt in bash?
layout: post
author: jungwook
category: bash
tags:
- bash
- git
---

### Reference page
[AutoComplete & Branch name](http://bit.ly/2fA0ZTV)
[Branch Color](http://bit.ly/2fzZ55r)

### bash prompt상의 git 사용 tip

1. Autocomplete

	+ git-completion 파일을 다운.
	<br>
    ```{.bash}
    wget https://raw.github.com/git/git/master/contrib/completion/git-completion.bash
    ```

	+ ~/.bashrc 파일에 하기 내용 추가.
	<br>
    ```{.bash}
    if [ -f ~/.git-completion.bash ]; then
    . ~/.git-completion.bash
    fi
    ```

2. Branch name on Bash

	+ git-prompt.sh 파일을 다운.
	<br>
    ```{.bash}
    wget https://raw.github.com/git/git/master/contrib/completion/git-prompt.sh
    ```

	+ ~/.bashrc 파일에 하기 내용 추가.
	<br>
    ```{.bash}
    if [ -f ~/.git-prompt.bash ]; then
    . ~/.git-prompt.bash
    fi
    ```

3. Branch Name 수정 유무에 따른 색 변경
	<br>
    ```{.bash}
    n=`tput setaf 6`
    c_red=`tput setaf 1`
    c_green=`tput setaf 2`
    c_sgr0=`tput sgr0`

    parse_git_branch ()
    {
       if git rev-parse --git-dir >/dev/null 2>&1
       then
          gitver=$(git branch 2>/dev/null| sed -n '/^\*/s/^\* //p')
       else
          return 0
       fi
       echo -e $gitver
    }

    branch_color ()
    {
       if git rev-parse --git-dir >/dev/null 2>&1
       then
          color=""
          if git diff --quiet 2>/dev/null >&2
          then
             color="${c_green}"
          else
             color=${c_red}
          fi
       else
          return 0
       fi
       echo -ne $color
    }

    export PS1='\u@\h\[${c_sgr0}\]:\W\[${c_sgr0}\] (\[$(branch_color)\]$(parse_git_branch)\[${c_sgr0}\])\$ '
    ```

  4. My .bashrc
	<br>
    ```{.bash}
    if [ -f ~/.git-completion.bash ]; then
    . ~/.git-completion.bash
    fi

    if [ -f ~/.git-prompt.sh ]; then
    c_cyan=`tput setaf 6`
    c_red=`tput setaf 1`
    c_green=`tput setaf 2`
    c_sgr0=`tput sgr0`

    branch_color ()
    {
       if git rev-parse --git-dir >/dev/null 2>&1
       then
          color=""
          if git diff --quiet 2>/dev/null >&2
          then
             color="${c_green}"
          else
             color=${c_red}
          fi
       else
          return 0
      fi
      echo -ne $color
    }  

    . ~/.git-prompt.sh
    fi

    if [ "$color_prompt" = yes ]; then
       PS1='${debian_chroot:+($debian_chroot)}\[\033[01;32m\]\u@\h\[\033[00m\]:\[\033[01;34m\]\w\[$(branch_color)\]\$(__git_ps1)\[${c_sgr0}\]\[\033[00m\]\$ '
    else
       PS1='${debian_chroot:+($debian_chroot)}\u@\h:\w\[$(branch_color)\]$(__git_ps1)\[${c_sgr0}\]\$ '
    fi
    ```
