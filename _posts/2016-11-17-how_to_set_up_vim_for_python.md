---
published: true
title: how to set up a vim for python?
layout: post
author: jungwook
category: vim
tag:
- vim
- python
---

I use below link

https://github.com/ets-labs/python-vimrc

I changed two thing for this

" python executables for different plugins
let g:pymode_python='python'
let g:syntastic_python_python_exec='python'
let g:jedi#force_py_version=2

If I use python3 so I changed below

" python executables for different plugins
let g:pymode_python='python3'
let g:syntastic_python_python_exec='python'
let g:jedi#force_py_version=3

