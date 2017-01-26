---
published: true
title: How to set git prompt in bash?
layout: post
author: jungwook
category: GIT
tags:
- bash
- git
---
### Reference page
[AutoComplete & Branch name](http://bit.ly/2fA0ZTV)
[Branch Color](http://bit.ly/2fzZ55r)

### bash prompt상의 git 사용 tip

### 1. Autocomplete

+ git-completion 파일을 다운.

```{.bash}
wget https://raw.github.com/git/git/master/contrib/completion/git-completion.bash
```

+ ~/.bashrc 파일에 하기 내용 추가.

```{.bash}
if [ -f ~/.git-completion.bash ]; then
. ~/.git-completion.bash
fi
```

### 2. Branch name on Bash

+ git-prompt.sh 파일을 다운.

```{.bash}
wget https://raw.github.com/git/git/master/contrib/completion/git-prompt.sh
```

+ ~/.bashrc 파일에 하기 내용 추가.
```{.bash}
if [ -f ~/.git-prompt.bash ]; then
. ~/.git-prompt.bash
fi
```

### 3. Branch Name 수정 유무에 따른 색 변경

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

### 4. My .bashrc

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

> 주의사항!
>
> wget으로 git-prompt.bash, git-completion.bash 두 파일을 받아오면 앞에 점이 없다. 따라서 wget 이후에 파일명에 .을 추가하거나 아니면 스크립트에서 해당 내용을 제거해야 한다. 할 때 잘하자!

### 5. 추가 설정

기본적인 user name, email 등의 설정 값과 log를 볼 때 visual하게 보이도록 하는 설정은 아래와 같다.

```{.bash}
#이름 설정 추가
$> git config --global user.name jungwook
#이메일 설정 추가
$> git config --global user.email email@email.com
#log visual
$> git config --global alias.lg "log --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit --date=relative"
```